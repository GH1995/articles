---
title: Install Xmind in Ubuntu 17.10
date: 2018-03-04 21:37:36
tags: Linux
---

{% asset_img Xmind.png Xmind %}

For icon, you can get the icon from {% asset_link xmind.svg here%} , and move it to `/usr/share/icons/hicolor/scalable/apps/`.

Create the `Xmind.desktop` in `/usr/share/applications`:
```
[Desktop Entry]
Comment=Create and share mind maps
Exec=/opt/xmind/XMind_amd64/XMind %F
Name=Xmind
Terminal=false
Type=Application
Categories=Office;
Icon=/usr/share/icons/hicolor/scalable/apps/xmind.svg
```
