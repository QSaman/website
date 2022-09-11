---
title: Kodi
taxonomy:
    category: docs
highlight:
    enabled: true
---

# Useful Kodi Addons

You can find some great addons on[Addons4Kodi](https://www.reddit.com/r/Addons4Kodi/).

## Subtitle
* [a4kSubtitles](https://a4k-openproject.github.io/a4kSubtitles/)

## Program
* [Yatse](https://yatse.tv/wiki/yatse-kodi-addon)
* Trakt

## PVR Clients
* [IPTV Simple Client](https://github.com/kodi-pvr/pvr.iptvsimple/)

## Services

* [SponsorBlock](https://github.com/siku2/script.service.sponsorblock)

## Video

### Youtube

* To add Personal YouTube API key follow this [tutorial](https://forum.kodi.tv/showthread.php?tid=267160&pid=2299960#pid2299960)
* The YouTube API no longer supports the full use of Watch Later and History preventing correct syncing and editing. For a workaround follow this [tutotial](https://forum.kodi.tv/showthread.php?tid=267160&pid=2299963#pid2299963). Basically you add a new playlist (e.g. "Kodi watch later") and tell YouTube plugin to use this playlist as watch-later. You can also tell to the plugin to remove videos from it after you watched them.

### Enabling [Dolby Vision](https://en.wikipedia.org/wiki/Dolby_Vision)

At the time of writing, Kodi internal player doesn't support the proprietary Dolby Vision; to fix this issue, one way is to install [Just Video Player](https://play.google.com/store/search?q=Just%20Video%20Player&c=apps) from Google Play and force Kodi to use this external player for Dolby Vision files. Please note that only Dolby Vision MKV files can be rewinded and forwarded using Just Video Player while that is not possible with Dolby Vision MP4 files. For more information refer to this [post](https://www.reddit.com/r/Addons4Kodi/comments/s0d5sr/dolby_vision_buffering_fix_for_kodi/).

After installing "Just Video Player", create a file named `playercorefactory.xml` and copy it into `/Android/data/org.xbmc.kodi/files/.kodi/userdata/`. The content of `playercorefactory.xml` should be:

```xml
<playercorefactory>
    <players>
        <player name="Just Player" type="ExternalPlayer" audio="false" video="true">
            <filename>com.brouken.player</filename>
            <hidexbmc>true</hidexbmc>
            <playcountminimumtime>120</playcountminimumtime>
        </player>
    </players>
    <rules action="prepend">
        <rule internetstream="true">
            <rule filename=".*[.]DV[.].*|.*\sDV\s.*|.*[.]Dv[.].*|.*\sDv\s.*|.*[.]dv[.].*|.*\sdv\s.*|.*D[/]VISION.*|.*\sDOVI\s.*|.*[.]DOVI[.].*|.*\sDoVi\s.*|.*[.]DoVi[.].*|.*\sDovi\s.*|.*[.]Dovi[.].*|.*\sdovi\s.*|.*[.]dovi[.].*|.*\sDOVi\s.*|.*[.]DOVi[.].*" player="Just Player" />
        </rule>
        <rule video="true" player="dvdplayer" />
    </rules>
</playercorefactory>
```


