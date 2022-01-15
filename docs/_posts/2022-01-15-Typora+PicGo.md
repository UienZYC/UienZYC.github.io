---
layout: post
title:  "如何配置Typora + PicGo"
date:   2022-01-15 12:21:00 +0800
categories: jekyll update
---
# 如何配置Typora + PicGo（markdown图床工具）

_UienZYC_

_2022.1.15_

+++

首先感谢以下链接及其作者，它们的文章带给我很大帮助。

+ Typora+PicGo，最好用的Markdown+最好用的图床工具！https://blog.csdn.net/bruce_6/article/details/104821531
+ PicGohttps://picgo.github.io/PicGo-Doc/

## 1. 为什么要用图床？

以下是我个人的理解。

markdown文本实质为一串代码，通过markdown阅读器据代码翻译出排版和公式等。故在markdown文件中不包含图片。

md呈现图片的方式是在代码中写入图片的路径（本地路径或网络地址），阅读器据路径加载图片。若路径失效，md阅读器则无法显示图片。

当md文件上传到网络后，本地图片的地址失效。为了解决这个问题，需要将图片上传到网络，并使用图片在网络的地址。

图床（Image Hosting Service）即储存图片的服务器，类似一个云盘，但只放图片，并获得一个图片在网络的链接，其在md文件中即可查看，有利于含图片md文件的共享。

常用图床包括SM.MS图床，GitHub图床等。本文以GitHub图床为示例。

## 2. 用到的工具

+ PicGo（图床工具，将本地图片上传至图床并转换为对应链接）下载地址：https://github.com/Molunerfinn/PicGo/releases（windows选择下载.exe文件）
+ Typora（md编辑器，支持与PicGo的联动，需要版本0.9.84及以上）
+ 你的GitHub账号

## 3. 让我们开始

### GitHub中的设置

1. 建立一个新仓库!![](https://raw.githubusercontent.com/UienZYC/mytuchuang/main/image-20220115114826144.png)

   然后![image-20220115120127520](https://raw.githubusercontent.com/UienZYC/mytuchuang/main/github_settings.png)
   在左侧栏点击Developer settings，Personal access tokens
   点击Generate new token
   注意给repo打勾如图![image-20220115120640480](https://raw.githubusercontent.com/UienZYC/mytuchuang/main/%E7%BB%99repo%E6%89%93%E5%8B%BE.png)
   点击最下方绿色的Generate token，获得只能在该处看见一次的一个字符串，将该token其复制到本地备用
2. PicGo中的设置
   PicGo设置->设置Server：打开server，参数默认即可
   点击图床设置->GitHub图床
   设定仓库名：用户名/仓库名
   设定分支名：main即可
   设定Token：此处复制刚才生成的token字符串
   （以下可选操作）
   PicGo设置-> 打开“上传前重命名”：由此修改上传到图床中的名称，便于在图床相册中查找。
3. Typora中的设置
   文件->偏好设置->图像->如下图配置![image-20220115121630714](https://raw.githubusercontent.com/UienZYC/mytuchuang/main/Typora%E4%B8%AD%E5%85%B3%E4%BA%8EPicGo%E7%9A%84%E9%85%8D%E7%BD%AE.png)
   点击“验证图片上传选择”，测试是否成功

## 4. 享受

