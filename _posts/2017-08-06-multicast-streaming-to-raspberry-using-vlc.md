---
title: Streaming multicast video to Raspberry Pi (from VLC)
tags: vscode, raspberrypi, debug, vlc
---

> I am starting new home automation project with RPi, and will try to cover some interesting aspects of this development here.

One of the features of my project is many-to-many streaming of video content on LAN.
So to do a stress test of RPi i wanted to set up a simple stock (no custom code) environment to prove that it will work.

I am using omxplayer and VLC (on windows) to set this up:
<!--break-->
## VLC setup for multicast streaming
This is not as obvious as it seems. You have to do File-Stream in VLC, and then select "RTP Audio/Video Profile". Then click "add" button and type your multicast address.

I have used 224.255.1.1 as my multicast address (name is optional). You can optionally enable transcoding, and then it generates VLC config for you, for example like this:
```
:sout=#transcode{vcodec=h264,vb=208,scale=0.25,acodec=mp4a,ab=32,channels=2,samplerate=22050}:rtp{dst=224.255.1.1,port=5004} :sout-keep
```
Here i am transcoding my video to reduce network traffic, and broadcasting via that address.

To convert this into a bat file for easy re-runnability, you have to map some parameters out like so:
```
"C:\Program Files (x86)\VideoLAN\VLC\vlc.exe" -vvv -I rc "d:\video\Aruba\aruba.mp4" --sout="#transcode{vcodec=h264,vb=208,scale=0.25,acodec=mpga,ab=128,channels=2,samplerate=22050}:rtp{dst=224.255.1.1,port=5004,sap,name=vlc,sdp=file:///C:/vlc.sdp}" --sout-keep --loop
```
## Anouncing your stream
Note that I also added another parameter here. This sdp parameter is critical for network streaming - it generates stream description file that raspberry will need to play the file correctly. Upload it to your RPi and move to next step.

> Technically sdp files can also be downloaded from unicast addresses, using SAP protocol. However, i couldn't get this to work with VLC - even VLC-to-VLC didn't work out of the box. Will try again with actual code, instead of VLC.

## Raspberry Pi setup to receive content
On the raspberry side, there are several players that you could use to receive RTP video. However, only omxplayer is completely raspberry hardware aware. It also uses ffmpeg with Broadcom-optimized drivers, that use hardware decoding in the video card.

The command to play network stream is
```
omxplayer --threshold 1 --loop --live --display 4 --win 0,0,240,135 vlc.sdp &
```
This is where the vlc.sdp file goes. If you tried playing the URL directly using rtp://@224.255.1.1:5004 you would only get sound - ffmpeg can only guess sound codec, but not video codec correctly.

Note that I reduced threshold - there's no buffering in multicast LAN streaming, packets just get lost. So this speeds up the initial video. 

Display parameter varies - not really documented anywhere, but 4 is for native touchscreen that I use, and 5 is for HDMI output.

> Looks like omxplayer can't do the sap:// URLs either, and just gives up immediately. ffplay does document this feature though. I may not be using SAP in my code though, so didn't investigate.

## Perf
So in the end I was able to run 8 streams simultaniously. It uses about 10% of CPU, the rest is in the GPU. I did increase GPU RAM to 256Mb using raspi-config. It's possible to increase it more by editing config file, but at this point you are compromising your regular RAM, as it's shared.

```
omxplayer --threshold 1 --loop --live --display 4 --win 0,0,240,135 vlc.sdp &
omxplayer --threshold 1 --loop --live --display 4 --win 0,135,240,270 vlc.sdp &
omxplayer --threshold 1 --loop --live --display 4 --win 0,270,240,405 vlc.sdp &
omxplayer --threshold 1 --loop --live --display 4 --win 240,0,480,135 vlc.sdp &
omxplayer --threshold 1 --loop --live --display 4 --win 240,135,480,270 vlc.sdp &
omxplayer --threshold 1 --loop --live --display 4 --win 240,270,480,405 vlc.sdp &
omxplayer --threshold 1 --loop --live --display 4 --win 480,0,720,135 vlc.sdp &
omxplayer --threshold 1 --loop --live --display 4 --win 480,135,720,270 vlc.sdp &
```

This streams beautifully and proves the concept.

[![screenshot](/images/IMG_20170815_225633_min.jpg)](/images/IMG_20170815_225633.jpg)


