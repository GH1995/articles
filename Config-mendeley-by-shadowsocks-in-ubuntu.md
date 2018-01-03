---
title: Config mendeley by shadowsocks in ubuntu
date: 2018-01-03 11:40:46
tags:
    - Linux
---

# add shadowsocks-qt5 ppa source

```
sudo add-apt-repository ppa:hzwhuang/ss-qt5
sudo apt-get update
sudo apt-get install shadowsocks-qt5
```

# mendeley config

use default port 1080, `tools->options->Connection`

{% asset_img mendeley.png mendeley %}
