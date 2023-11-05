---
layout: post
title:  Debian Linux on Thinkpad L490
---

Trackpoint & touchpad and wifi were (hopefully) the only things that didn’t work right after installation of Debian Buster on my Lenovo Thinkpad L490.


# Wifi
It didn’t work during installation so I installed using an eth cable and then I had to:

- add `non-free` and `contrib` repositories to `/etc/apt/sources.list`: \
 `deb http://deb.debian.org/debian buster main contrib non-free`
- install `firmware-linux-nonfree` and `firmware-iwlwifi` packages

# Trackpoint & touchpad
To make it work you need to add a parameter to `psmouse` module. To test if it works:

```sh
sudo rmmod psmouse
sudo modprobe psmouse proto=imps
```

And to make it permanent:

```sh
echo options psmouse proto=imps | sudo tee /etc/modprobe.d/mouse.conf
sudo update-initramfs -c -k all
sudo update-grub
sudo reboot
```

# Fingerprint reader
I haven’t managed to make it work.


# Cooling and Battery Life

There is some problem with cooling or maybe just with some sensors which I still haven’t figured out. `dmesg` has messages like this quite often:

```
[69668.650479] mce: CPU0: Package temperature above threshold, cpu clock throttled (total events = 3882682)
[69668.650481] mce: CPU2: Package temperature above threshold, cpu clock throttled (total events = 3882682)
[69668.650482] mce: CPU6: Package temperature above threshold, cpu clock throttled (total events = 3882682)
[69668.651560] mce: CPU5: Package temperature/speed normal
[69668.651561] mce: CPU1: Package temperature/speed normal
[69668.651562] mce: CPU4: Package temperature/speed normal
[69668.651563] mce: CPU0: Package temperature/speed normal
```

The battery life is definitely not the advertised 12 hours. Or the estimate in Gnome is wrong. It says 76% after being charged for days and 3 hours remaining (!) when unplugged. But I havent done any proper testing.

`s-tui` output with Gnome running, a few opened Chrome windows and an idle IntelliJ Idea:

![screenshot of a terminal with s-tui app running](/assets/2019-thinkpad.webp)
