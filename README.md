uvcvideo
========

uvcvideo driver with still image capture support

* The base is kernel 2.6.35.7

* Picked some fixes from upstrea

* Make it possible to build out side the kernel source tree.

  $> make

* Also and build in your Android enviroment with

  $> make -f Makefile.android

Background
==========

What's still image capture in UVC?

Refer to the USB Video Class 1.1 spec, 2.4.2.4 Still Image Capture, page 14.

End user's view: it's means you can capture a still image with different
size to the current preview size. e.g preview in 800x600, but you wanna
still image in 1600x1200.

Technical view: it's means application don't need stream off the current
video stream (preview stream), and send a trigger control command to camera,
to get a still image frame via the current preview stream pipe line, after
still image sent, camera will auto switch back to preview stream, and
application continue to show preview stream.

Why we need still image capture mode?

* Capture still image with same size as the video/preview stream is not fit
  to HD camera (> 2M).

* Stream off before capture and stream on after capture may cost more than
  5 seconds, because some uvc camera need a bit long time to warm up and
  send out first frame.

* Big size still image requires more band width and system resource, it's
  not wise to preview with a big size.

* Power consume (bigger size more power consumed)

Implementation
==============

* Use a magic number to implement REQBUFS/QUERYBUF/QBUF/DQBUF/S_FMT for
  still image buffers management, format set..etc. While V4L2 has no special
  ioctl for still image, I did it this way, maybe new private ioctrl or new
  subdev is better approach.

* Add a new still queue for app to get still image buf with same API.

* Pares still format info in the original descriptor scan code.

* Parse still image frame in the decode_isoc start/data/end func (decode_bulk
  not support yet), and send out finished buffer with a frame.

User space tool
===============

I also customized the famous v4l2 capture.c to do still image capture, and
I've successfully captured still image on a live preview stream (I wrote a
tool to convert yuv422 raw data into png, and use it to verify the captured
still image).

My uvc camera is a Vimicro vc0346 (UVC ic) + Aptina mt9p111 5M sensor.

FAQ
===

How to identify if your uvc camera support still image capture?

lsusb -v -d xxxx:xxxx

Find STILL_IMAGE_FRAME in the output, if found, it does, else not.

Atuhors
=======

* Yang Hong - Implement still image capture
* Liu Shouyong - Tips and initial still probe/commit code

Links
=====

* V4L2 API - http://linuxtv.org/downloads/v4l-dvb-apis/
* Linux UVC driver & tools - http://www.ideasonboard.org/uvc/
* Maillist - linux-uvc-devel@lists.sourceforge.net
* Maillist subscribe - https://lists.sourceforge.net/lists/listinfo/linux-uvc-devel
* USB Video Class Specs - http://www.usb.org/developers/devclass_docs/USB_Video_Class_1_1_090711.zip
