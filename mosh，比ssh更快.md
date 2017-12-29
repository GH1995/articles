---
title: mosh，比ssh更快
date: 2017-12-29 21:10:43
tags:
    - Linux
---

关于 mosh

## 端口

mosh 默认采用 `60001` 端口，第二个链接依次递增。

```
iptables -I INPUT -p udp --dport 60001 -j ACCEPT
```

## 字符集问题

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
