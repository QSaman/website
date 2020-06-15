---
title: mpv
taxonomy:
    category: docs
---

## Playing Stream Websites through mpv

You need [youtube-dl](https://rg3.github.io/youtube-dl/). After installing it write the following command:

```
mpv --ytdl --ytdl-format=best url
```

Note that you can run something like:

```
mpv --ytdl --ytdl-format="bestvideo[ext=webm]+bestaudio[ext=webm]" url
```

### CWTV

If you want to have subtitle:

```
youtube-dl --write-sub --skip-download url
```

The subtitle is in `dfxp` format which `ffmpeg` does not support. A simple solution is to use [netflix-to-srt](https://github.com/isaacbernat/netflix-to-srt) to convert it to `srt` extension. Note that it expects `xml` instead of `dfxp`. 

```
mv subtitle.dfxp subtitle.srt
to_srt.py
```

After you get `srt` subtitle:

```
mpv --ytdl --ytdl-format=best --sub-file=subtitle.srt url
```

## Playing a directory music recursively
If your mpv version is 0.9.0 or more use the following command:
```
mpv --shuffle --no-audio-display "Star Wars Music"
```
If it's below 0.9.0. Use the following command. We need absolute path, so we've used ```"`pwd`"```. For more information see the following [link](http://unix.stackexchange.com/questions/30367/how-can-i-retain-the-console-input-in-mplayer-when-reading-from-stdin)
```
find "`pwd`" -type f -iname '*.mp3' | mpv --shuffle --no-audio-display --playlist=/dev/fd/3 3<&0 < /dev/tty
```
Instead of file descriptors, we can use [process substitution](https://en.wikipedia.org/wiki/Process_substitution):
```
mpv --shuffle --no-audio-display --playlist <(find "$(pwd)" -type f -iname '*.mp3')
```
Playing only videos in a directory recursively (It can be very slow):
```
find "`pwd`" -type f -exec file --mime-type '{}' \; | sed -n 's!: video/[^:]*$!!p' | \
mpv --shuffle --no-audio-display --playlist=/dev/fd/3 3<&0 < /dev/tty
```
For more information visit this [site](http://unix.stackexchange.com/questions/106436/how-to-search-for-video-files-on-ubuntu). 
If you are using [Baloo](https://community.kde.org/Baloo) and you indexed the directory, you can search faster:
```
baloosearch -t Video -l1000 anakin |\
sed -r "s/\x1B\[([0-9]{1,2}(;[0-9]{1,2})?)?[mGK]//g" |\
cut -d ' ' -f4- |\
mpv --playlist=/dev/fd/3 3<&0 < /dev/tty
```
For more information visit this [site](http://unix.stackexchange.com/questions/111899/how-to-strip-color-codes-out-of-stdout-and-pipe-to-file-and-stdout)
## Subtitles
For playing a movie whose subtitle is in a subdirectory:
```
mpv --sub-paths=Subs a.mkv
```
For playing a movie whose subtitle file has a different name:
```
mpv --sub-file=a.srt b.mkv
```
## Audio
For playing a movie which has a external audio file:
```
mpv --audio-file=a.m4a v.mp4
```
For playing audio through different device (e.g. hdmi), First run below comamnd to see the list of recognized devices:
```
mpv --audio-device=help
```
  example output:
```
'alsa/hdmi:CARD=HDMI,DEV=0' (HDA Intel HDMI, HDMI 0/HDMI Audio Output)
```
  we need to run below command for hdmi:
```
mpv --audio-device='alsa/hdmi:CARD=HDMI,DEV=0' a.mkv
```
## DVDs
For playing a dvd movie with dvdnav menus:
```
mpv dvd://menu --dvd-device=a.iso
mpv dvd://menu --dvd-device=VIDEO_TS/
```
