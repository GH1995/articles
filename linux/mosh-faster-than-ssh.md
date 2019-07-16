---
title: 'mosh, faster than ssh'
tags:
  - Linux
categories:
  - linux
date: 2017-12-29 21:10:43
---


## port

mosh use the port of `60001`, then the secend increasely.

```
iptables -I INPUT -p udp --dport 60001 -j ACCEPT
```

## how to support Chinese

```
sudo apt install language-pack-zh-hant language-pack-zh-hans

sudo vim /etc/environment
LANG="zh_CN.UTF8"
LANGUAGE="zh_CN:zh:en_US:en"

sudo vim /etc/default/locale
LANG="zh_CN.UTF-8"
LANGUAGE="zh_CN:zh"

sudo reboot
```
