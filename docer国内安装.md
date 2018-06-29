---
title: docer国内安装
date: 2018-06-22 19:22:55
tags:
    - Linux
    - docker
---

```sh
sudo apt-get remove docker \
               docker-engine \
               docker.io
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository \
    "deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu \
    $(lsb_release -cs) \
    stable"
sudo apt-get update && sudo apt-get install docker-ce
sudo groupadd docker
sudo gpasswd -a ${USER} docker
sudo service docker restart
newgrp - docker
```
`/etc/docker/daemon.json`
```js
{
  "registry-mirrors": [
    "https://registry.docker-cn.com"
  ]
}
```
