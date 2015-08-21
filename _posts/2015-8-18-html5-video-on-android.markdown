---
layout: post
title:  "html5 video for android"
date:   2015-08-18 23:00:00
categories: HTML5 Video Android 视频编码
tags: Webfrontend
image: /assets/article_images/2015-01-25-webgl-shader/plexus.jpg
---


###html5 video for android

前一阵子搞html5 的video，为pc端已经操了很多心
然而，这只是开始....

pc上支持的视频格式主要是根据浏览器来定的
所以需要webm、ogv、mp4来保证，具体兼容性可以w3school查到，不多说了

```
<source src="1.webmv" type='video/webm'/>
<source src="2.ogv" type='video/ogv'/>
<source src="3.m4v" type='video/mp4'/>
<source src="1.mp4" type="video/mp4"/>
<source src="1.m4v" type='video/m4v'/>
```

移动端就比较奇葩了

因为android4.x及其以下的浏览器内核策略是，都得用系统的webkit核，所以首先webm和ogv已经死了，还剩下什么呢？![Alt text](/assets/article_images/html5-video/屏幕快照 2015-08-18 22.24.38.png)

于是我查了一下[android的文档有关媒体格式的支持](http://developer.android.com/guide/appendix/media-formats.html)

可以看到，mp4和3gp是比较好的选择，也是大家熟悉的内容。

###然而，不要高兴太早!

手机不像pc，系统支持的解码有限，光是格式对路是没有用的！
码率还得对才行！
鉴于现在国产手机主流rom和在售、在使用的都在4.2及其以下，所以说，VP8/9都不能用，H.265也不能用，只剩下H.264的MP4是可以使用的！[网上找了个样本视频](http://techslides.com/sample-webm-ogg-and-mp4-video-files-for-html5)

下图展示了一个真正能在广大移动端设备上播放的视频格式和编码压制信息
![Alt text](/assets/article_images/html5-video/屏幕快照 2015-08-18 22.37.30.png)
