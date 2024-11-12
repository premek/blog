---
layout: post
title:  "Share phone and Linux PC clipboard"
---

Clipboard sync is my favourite KDE Connect feature. 
Great for copy-pasting one-time passwords :)

<!--more-->

Install **KDE Connect** on your [phone](https://play.google.com/store/apps/details?id=org.kde.kdeconnect_tp)
and on your desktop. 
On Gnome you can use [GSConnect](https://extensions.gnome.org/extension/1319/gsconnect/).
Pair the two devices in the app. You can test the connection, for example by moving your PC mouse cursor from the phone ("Remote input").

Automatic clipboard access is disabled on newer Android for security reasons but there is a workaround in KDE Connect.
You will need [USB debugging](https://developer.android.com/studio/debug/dev-options) enabled on your phone.
(_You are now a developer!_) You don't need phone's root access for this method.

Install `adb` (part of [SDK Platform Tools](https://developer.android.com/tools/releases/platform-tools)) on your PC.

Connect your phone using USB. You can test the connection using the `adb devices` command. You should see at least one device connected:
```
List of devices attached
ZX11JJGGJS	device
```

Run the following commands:
```
adb -d shell pm grant org.kde.kdeconnect_tp android.permission.READ_LOGS;
adb -d shell appops set org.kde.kdeconnect_tp SYSTEM_ALERT_WINDOW allow;
adb -d shell am force-stop org.kde.kdeconnect_tp;
```

You might need to confirm something on the phone's screen.

The last command should stop kdeconnect on your phone,
and after you start it again and leave it running in the background the 
automatic clipboard sync should work.
I had to restart kdeconnect one more time.

