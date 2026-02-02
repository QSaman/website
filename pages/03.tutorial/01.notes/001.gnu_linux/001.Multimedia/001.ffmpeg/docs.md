---
title: ffmpeg
taxonomy:
    category: docs
---

### Concatenating vide files

#### Non-decoding approach

This method doesn't requires any decoding and ffmpeg concatenate the streams byte by byte. However, if there is mismatches, it fails. First create a file with the name of files to concatenate. For example:

```
cat files.txt
file 'file 1.mp4
file 'file 2.mp4'
```

then:

```
ffmpeg -f concat -safe 0 -i files.txt -c copy output.mp4
```

Sometimes it fails. For example:

```
[aost#0:1/copy @ 0x558032d2af00] Non-monotonic DTS; previous: 67814400, current: 36167680; changing to 67814401. This may result in incorrect timestamps in the output file.
[aac_adtstoasc @ 0x7f4fa0000f00] Error parsing ADTS frame header!
```
There is a warning and error here. First some vocabulary

* DTS: Decoding timestamp tells the decoder when to decode each frame. It should increase monotonically.
* PTS: presentation timestamp tells when to display it

The warning is now clear. The second error means that one file is in ADTS which not-compatible for mp4. That means we need to fix DTS and encode the audio:

```
ffmpeg -fflags +genpts -i "file 1.mp4" -c:v copy -c:a aac -b:a 125k normalized1.mp4
```

Note that 125k is used because `file 1.mp4` uses 125k. You can verify it by:

```
ffmpeg -i "file 1.mp4"
```

Now we can use normalized1.mp4 as before for concatenating.

#### Decoding approach

This is more flexible and it works with different codecs. However, it requires encoding again.

```
ffmpeg -i "file 1.mp4" -i "file 2.mp4" \
-filter_complex "[0:v][0:a][1:v][1:a]concat=n=2:v=1:a=1[v][a]" \
-map "[v]" -map "[a]" \
-c:v libx264 -crf 18 -preset medium \
-c:a aac -b:a 192k \
output.mp4
```

Here is the explanation for `filter_complex`:

```
[0:v]  → video stream from input #0  
[0:a]  → audio stream from input #0  
[1:v]  → video stream from input #1  
[1:a]  → audio stream from input #1
```

Then `n=2` means there are two segments (files) and each of them has one video and audio stream (`v=1:a=1`). Now we need to tell ffmpeg to use the filter complex output stream instead of input by using `-map`.

### Save a stream video

You can press F12 in your browser. Then go to Network tab and filter `m3u8` which are [HLS files](https://en.wikipedia.org/wiki/HTTP_Live_Streaming). Then copy the URL:

```
    ffmpeg -i https://*.m3u8 -c copy ~/output.mp4
```

### Capturing webcam & mic

For more information visit [this](https://trac.ffmpeg.org/wiki/Capture/Webcam) and [this](https://trac.ffmpeg.org/wiki/Capture/ALSA). First we need to determine the spec of Webcam and mic:

```
$ v4l2-ctl --list-devices

```

In my case I need to use `/dev/video0`. If you know that your webcam support higher resolution but it records in a lower one, visit this [site](https://superuser.com/questions/494575/ffmpeg-open-webcam-using-yuyv-but-i-want-mjpeg). In my case only MJPEG supports higher resolution. For audio use this:

```
$ arecord -l
```

Now we have enough information to record both video and audio:

```
$ ffmpeg -f v4l2 -input_format mjpeg -video_size 1280x720  -i /dev/video0 -f alsa -i hw:0 /tmp/output.mp4
```

If you just want to test the webcam but don't want to save it (in my case no audio):

```
$ ffplay -f v4l2 -input_format mjpeg -video_size 1280x720  -i /dev/video0
```

### Change frame rate

```
ffmpeg -i input.mp4 -r 30 output.mp4
```    

or

```
ffmpeg -i input.mp4 -vf fps=fps=30 output.mp4
```

For more information visit this [page](https://trac.ffmpeg.org/wiki/ChangingFrameRate)

### Change resolution and frame rate

```
ffmpeg -i input.mp4 -r 30 -vf scale=-1:1080 -acodec copy output.mp4
```

### Change resolution
```
    ffmpeg -i input -vf scale=1920:1080 output.mp4
```    
  *Note:* If you want to preserve aspect ration, You need to use something like:
```
    ffmpeg -i input -vf scale=1920:-1 output.mp4
```    
  If you want to have a good bitrate you can use something like the following line.
  Type `ffmpeg -h encoder=libx264` for more options.
```  
    ffmpeg -i input -vf scale=1920:-1 -c:v libx264 -preset veryslow -crf 18  output.mkv
```    
  For stereoscopic movies use the following command:
```  
    ffmpeg -i input input.mp4 -vf scale=-1:1800 -c:v libx264 -crf 13  output.mp4
```    
### Cut some part of a video
    for example cut from 00:29:00 and length of new movie is 60s. For more
    information see Seeking – FFmpeg.maff.
```    
    ffmpeg -ss 00:29:00 -i input.mp4 -t 60 -c copy output.mp4
```    
Cut some parts of a video and them concatenating them to form a single video file:
```
    ffmpeg -i input.mp4 -t 63 -c copy -avoid_negative_ts 1 cut1.webm
    ffmpeg -ss 00:29:00 -i input.mp4 -to 00:30:00 -c copy -copyts -avoid_negative_ts 1 cut2.webm
    ffmpeg -ss 00:50:00 -i input.mp4 -to 00:63:00 -c copy -copyts -avoid_negative_ts 1 cut3.webm
```
*Note:* I've got wrong durations when I used `-to`. So it's better to use `-t` instead:
```
    ffmpeg -i input.mp4 -t 63 -c copy -avoid_negative_ts 1 cut1.webm
    ffmpeg -ss 00:29:00 -i input.mp4 -t 60 -c copy -copyts -avoid_negative_ts 1 cut2.webm
    ffmpeg -ss 00:50:00 -i input.mp4 -t 780 -c copy -copyts -avoid_negative_ts 1 cut3.webm
```
Then you need to create a file e.g. `mylist.txt`:
```
# This is a comment
file './cut1.webm'
file './cut2.webm'
file './cut3.webm'
```
Then use the following command to merge them:
```
ffmpeg -f concat -safe 0 -i list.txt -c copy output.webm
```
### Audio extraction
If you want to extract audio from a video without encoding, first run the following
  command to find out the correct extension.
```  
    ffmpeg -i input.mkv
```    
  Sample output:
```  
    Input #0, matroska,webm, from...
    Stream #0:0(und): Video:...
    Stream #0:1(eng): Audio:...
```

  So the extension of audio should be webm. Then run the following command.   
```  
    ffmpeg -i input.mp4 -c:a copy -vn -sn output.webm
  -vn means "no audio". -sn means "no subtitle"
```

### File information
```
    ffprobe -show_streams foo.webm
```    
  If you want to see only video information:
```  
    ffprobe -show_streams -select_streams v foo.webm
```    
### Rotating
If you want to rotate a video file without re-encoding, you can change the metadata of the container using the following command:

```
ffmpeg -i input.mp4 -c copy -metadata:s:v:0 rotate=90 output.mp4
```
You can check the metadata using the following command:
```
ffprobe -v quiet output.mp4 -show_streams | grep rot
```

### 3D Filter

#### Convert a side by side 3D movie to 2D

```
ffmpeg -i input -vf stereo3d=sbs2l:ml test.mp4
```
* `sbs2l`: side by side parallel with half width resolution (left eye left, right eye right)
* `ml`: mono output (left eye only)

For more information visit [this](https://trac.ffmpeg.org/wiki/Stereoscopic) and [this](https://ffmpeg.org/ffmpeg-filters.html#stereo3d)
