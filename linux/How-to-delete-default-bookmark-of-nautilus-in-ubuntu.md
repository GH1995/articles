---
title: How to delete default bookmark of nautilus in ubuntu ?
tags:
  - Linux
categories:
  - linux
date: 2018-01-02 18:55:10
---

The default bookmark is built from `~/.config/user-dirs.dirs` and `/etc/xdg/user-dirs.defaults`. So we must comment the bookmark that you want to delete in these files. Then logout and relogin.
