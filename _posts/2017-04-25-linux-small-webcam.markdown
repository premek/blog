---
layout: post
title:  The smallest webcam under Linux
---

I just bought this small & cheap camera from AliExpress and tested it under Linux. It has no name or brand, the manual says Mini camera, The smallest camera in the world. It can take pictures and video, has a wierd USB connector (probably [UC-E6](http://connector.pinoutguide.com/8_pin_UC-E6_like-mini-usb_proprietary/), ships with a cable) and can be used as a webcam.

It cost me about $12 together with a 8GB microSD card.

![photo of the camera](/assets/2017-webcam1.webp)

When you connect the cable, the SD card is mounted with no problem.

To use it as a webcam (I think I need to push the button on the cam after connecting the USB) I needed to reconfigure the `uvcvideo` module with a `quirks=2` option:

```sh
sudo rmmod uvcvideo
sudo modprobe uvcvideo quirks=2
```

Note you can add this parameter to `/etc/modules` or `/etc/modprobe.d/`.

Then I can use `mplayer`, `mpv`, `cheese` or any other application to test it:

```sh
mplayer tv:// --tv-device=/dev/video0
mpv av://v4l2:/dev/video0
```


![photo of the camera](/assets/2017-webcam2.webp)

Some more information about the camera below:

```
$ sudo dmesg
[2031649.188998] usb 1-2: new high-speed USB device number 68 using xhci_hcd
[2031649.329890] usb 1-2: New USB device found, idVendor=1b3f, idProduct=2002
[2031649.329892] usb 1-2: New USB device strings: Mfr=1, Product=2, SerialNumber=0
[2031649.329894] usb 1-2: Product: GENERAL - UVC 
[2031649.329895] usb 1-2: Manufacturer: GENERAL
[2031649.330601] uvcvideo: Found UVC 1.00 device GENERAL - UVC  (1b3f:2002)
[2031649.330681] uvcvideo: UVC non compliance - GET_DEF(PROBE) not supported. Enabling workaround.
[2031649.330911] uvcvideo 1-2:1.0: Entity type for entity Processing 5 was not initialized!
[2031649.330913] uvcvideo 1-2:1.0: Entity type for entity Selector 4 was not initialized!
[2031649.330915] uvcvideo 1-2:1.0: Entity type for entity Camera 1 was not initialized!
[2031649.331054] input: GENERAL - UVC  as /devices/pci0000:00/0000:00:14.0/usb1/1-2/1-2:1.0/input/input438
$ lsusb 
Bus 001 Device 072: ID 1b3f:2002 Generalplus Technology Inc. 808 Camera #9 (web-cam mode)
$ v4l2-ctl -d 2 --all
Driver Info (not using libv4l2):
 Driver name   : uvcvideo
 Card type     : GENERAL - UVC 
 Bus info      : usb-0000:00:14.0-2
 Driver version: 4.9.6
 Capabilities  : 0x84200001
  Video Capture
  Streaming
  Extended Pix Format
  Device Capabilities
 Device Caps   : 0x04200001
  Video Capture
  Streaming
  Extended Pix Format
Priority: 2
Video input : 0 (Camera 1: ok)
Format Video Capture:
 Width/Height      : 640/480
 Pixel Format      : 'MJPG'
 Field             : None
 Bytes per Line    : 0
 Size Image        : 614400
 Colorspace        : Default
 Transfer Function : Default
 YCbCr/HSV Encoding: Default
 Quantization      : Default
 Flags             : 
Crop Capability Video Capture:
 Bounds      : Left 0, Top 0, Width 640, Height 480
 Default     : Left 0, Top 0, Width 640, Height 480
 Pixel Aspect: 1/1
Selection: crop_default, Left 0, Top 0, Width 640, Height 480
Selection: crop_bounds, Left 0, Top 0, Width 640, Height 480
Streaming Parameters Video Capture:
 Capabilities     : timeperframe
 Frames per second: 30.000 (30/1)
 Read buffers     : 0
                     brightness (int)    : min=1 max=255 step=1 default=16 value=16
```

![an ad for the camera](/assets/2017-webcam3.webp)

# Update

After about two years of not using it very much the **BATTERY EXPLODED**.

I can still use it as webcam or when powered from USB.

