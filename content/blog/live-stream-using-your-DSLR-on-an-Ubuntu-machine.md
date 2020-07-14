---
title: "Live Stream Using Your DSLR on an Ubuntu Machine"
date: 2020-07-12T23:49:41+05:30
slug: ""
description: ""
keywords: [youtube, live-stream, dslr, ubuntu, ubuntu20, webcam, stream]
draft: true
tags: [livestream, photography, dslr, ubuntu, tech]
math: false
toc: false
---

* tldr;

```
sudo apt-get install gphoto2
sudo apt-get iinstall v4l2looback-utils ffmpeg
sudo modprobe v4l2loopback exclusive_caps=1 max_buffers=2

gphoto2 --auto-detect
gphoto2 --summary
gphoto2 --abilities
gphoto2 --capture-image-and-download

gphoto2 --stdout --capture-movie | ffmpeg -i - -vcodec rawvideo -pix_fmt yuv420p -threads 0 -f v4l2 /dev/video0

# GPU acceleration
gphoto2 --stdout --capture-movie | ~/src/ffmpeg/ffmpeg -hwaccel cuvid -c:v mjpeg_cuvid -i - -vcodec rawvideo -pix_fmt yuv420p -threads 0 -f v4l2 /dev/video0

https://medium.com/nerdery/dslr-webcam-setup-for-linux-9b6d1b79ae22
```