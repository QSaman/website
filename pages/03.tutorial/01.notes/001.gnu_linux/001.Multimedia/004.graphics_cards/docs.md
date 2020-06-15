---
title: Graphics Cards
taxonomy:
    category: docs
---

# Definition
* VA-API (Video Acceleration API): The VA API is to be implemented by device drivers to offer end-user software, such as VLC media player or GStreamer, access to available video acceleration hardware. VA-API is developed by Intel
* VDPAU (Video Decode and Presentation API for Unix): a competing API designed by NVIDIA, can potentially also be used as a backend for the VA API. If this is supported, any software that supports VA API then also indirectly supports a subset of VDPAU

For more information [Hardware Video Acceleration](https://wiki.archlinux.org/index.php/Hardware_video_acceleration)

# Intel Graphics Card
To see the name of the architecture, run the following command. Note that `vainfo` is for verifying the support of VA API:
 
```
vainfo
```
or more precisely:

```
vainfo 2> /dev/null | grep -i "driver version"
```

If you want to see the exact model of graphics card:

```
lspci | grep -i vga
```

For example mine is Broadwell. It completely supports decoding VP8, but it partially supports VP9. In other words, to decode VP9 by both CPU and Graphics Card I need to install `libva-intel-hybrid-driver` in Fedora. After that `vainfo` shows VP9. In Fedora 29 when video player tries to do hardware-decoding, it hangs. The solution is to exclude VP9 for hardware decoding (for `mpv` see `--hwdec-codecs` flag) or remove `libva-intel-hybrid-driver`.

# Nvidia Graphics Card

To verify VDPAU run the following command:

```
vdpauinfo
```
