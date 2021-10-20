---
layout: post
title:  "Headless RPI（无头配置树莓派方法）"
date:   2021-09-07 00:00:01 +0800
categories: jekyll update
typora-copy-images-to: ..\_image
typora-root-url: ..
---

### Headless RPI（无头配置树莓派方法）

_本文涉及如何在树莓派只有一根电源线的情况下用PC连接它_

_UienZYC 2021.9.7_

***

#### 设置静态IP

_每次树莓派连接WiFi后，路由器分给它一个IP地址不尽相同，为了连接方便，我们需设置树莓派使得每次路由器分给它同样的IP地址_

VNC连接后，右击网络

![HeadlessRPI-1]({{ "/_image/HeadlessRPI-1.png" | prepend: site.baseurl }})
![HeadlessRPI-2](/_image/HeadlessRPI-2.png)

如图设置后重启`sudo reboot`

之后就可以使用这个IP（.103）来连接了。回到路由器中查看，此时分配的是103.

![HeadlessRPI-3 ](/_image/HeadlessRPI-3 .png)
