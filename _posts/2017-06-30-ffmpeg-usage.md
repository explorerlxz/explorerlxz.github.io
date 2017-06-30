---
layout: post
title:  "ffmpeg(avconv)使用说明"
date:   2017-06-19
---

手机录制一个两分钟的视频，一般大小都在两三百兆，今年几乎每个月录制的视频加起来接近20G，不仅影响硬盘和手机空间，也不方便传输，于是打算以后录制的视频统统剪辑重新编码一下。




```
ffmpeg -ss 00:00:15 -t 00:01:10 -i bridge.mp4 -vcodec copy -acodec copy bridge-output.mp4
ffmpeg -i bridge-output.mp4  -c:v libx264 -crf 32 bridge-crt32.mp4
```

执行完上述命令后，视频的体积变成原来的三十分之一，视频质量并没有降低很多，我感觉挺满意。

```
[xwr@arch jianji]$ ls -l bridge*
-rwxrwxrwx 1 xwr users   6890233 Jun 30 10:16 bridge-crt32.mp4
-rwxrwxrwx 1 xwr users 227158036 Jun 29 07:12 bridge.mp4
-rwxrwxrwx 1 xwr users 176375045 Jun 30 10:00 bridge-output.mp4
[xwr@arch jianji]$
```
