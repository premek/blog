---
layout: post
title:  A "guitar" powered by Raspberry Pi & ChucK
---
I made me an electric guitar from a piece of wood and a few of the cheapest guitar parts. This is how to connect it to Raspberry Pi and how to setup ChucK to be able to program an effect in it.


![photo of my guitar](/assets/2016-guitar.webp)

I used an old Raspberry Pi with 1$ USB sound card connected to it and headphones and my “guitar” connected to the sound card.

On the Raspberry I’m running Raspbian so installing chuck was easy:

```
$ sudo aptitude install chuck
```

Now I needed to configure chuck input and output to use my USB sound card which is done by `--dacN` and `--adcN` command line options.

> **dac** means digital-to-analog converter and represents audio **output (headphones)**
>
> **adc** is analog-to-digital converted and represents audio **input (mic)**

Default `chuck` command uses `jack` which I don’t have installed so I’m using `chuck.alsa` command everywhere. `--probe` option lists available devices that can be used.

```
$ chuck.alsa --probe
```

First I found the one used for output (dac — the headphones) using the following little ChucK script which just plays sine wave for 3 seconds. In my case it was device number 3.

```
SinOsc s => dac;
3::second => now;
```

```
$ chuck.alsa --dac3 test-dac.ck
```

To test the input device setting (adc) I used another ChucK script which just repeats to the headphones what it hears on the mic. When looking for adc device in the `--probe` output, don’t get confused by the “dac” in the titles, look for a device which has e.g. `input channels = 1`.

```
adc => dac;
3::second => now;
```

```
chuck.alsa --dac3 --adc3 --in1 --bufsize100 adc_to_dac.ck
```

The `--in1` parameter should match the number of input channels in `chuck.alsa --probe` output.

The `--bufsize` parameter is used to lower the latency.


Now we should have ChucK running and configured to play whatever we play on the guitar. Now we can try to create our custom guitar effect. The script below is just an example.

```
adc => NRev r => Gain AM => SinOsc od => dac;
3 => AM.op;
1 => od.sync;
day => now;
```

<iframe width="100%" height="166" scrolling="no" frameborder="no" allow="autoplay" src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/299940814&color=%23ff5500&auto_play=false&hide_related=false&show_comments=true&show_user=true&show_reposts=false&show_teaser=true"></iframe><div style="font-size: 10px; color: #cccccc;line-break: anywhere;word-break: normal;overflow: hidden;white-space: nowrap;text-overflow: ellipsis; font-family: Interstate,Lucida Grande,Lucida Sans Unicode,Lucida Sans,Garuda,Verdana,Tahoma,sans-serif;font-weight: 100;"><a href="https://soundcloud.com/premek-v" title="Premek Vyhnal" target="_blank" style="color: #cccccc; text-decoration: none;">Premek Vyhnal</a> · <a href="https://soundcloud.com/premek-v/test-01" title="Test 01" target="_blank" style="color: #cccccc; text-decoration: none;">Test 01</a></div>


If you are satisfied with your script you can add a line like `chuck.alsa … g.ck &` to `/etc/rc.local` to run it automatically after booting. Then you can just connect the raspberry to power at any time and you can play.

If you want to *record* what you play, you can use the rec.ck program which comes with ChucK examples or you can download it from here: http://chuck.cs.princeton.edu/doc/examples/basic/rec.ck

To use it, just run chuck with both your program and rec.ck. Add the filename you want to record to after the rec.ck like this:

```
chuck.alsa --dac3 --adc3 --in1 --bufsize100 g.ck rec.ck:file.wav
```

<iframe width="100%" height="166" scrolling="no" frameborder="no" allow="autoplay" src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/299951364&color=%23ff5500&auto_play=false&hide_related=false&show_comments=true&show_user=true&show_reposts=false&show_teaser=true"></iframe><div style="font-size: 10px; color: #cccccc;line-break: anywhere;word-break: normal;overflow: hidden;white-space: nowrap;text-overflow: ellipsis; font-family: Interstate,Lucida Grande,Lucida Sans Unicode,Lucida Sans,Garuda,Verdana,Tahoma,sans-serif;font-weight: 100;"><a href="https://soundcloud.com/premek-v" title="Premek Vyhnal" target="_blank" style="color: #cccccc; text-decoration: none;">Premek Vyhnal</a> · <a href="https://soundcloud.com/premek-v/test-02" title="Test 02" target="_blank" style="color: #cccccc; text-decoration: none;">Test 02</a></div>

