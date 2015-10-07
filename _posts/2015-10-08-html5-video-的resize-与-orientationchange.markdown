---
layout: post
title:  "html5 移动端 的resize 与 orientationchange"
date:   2015-10-08 00:01:04
categories: HTML5 旋转 resize orientationchange
tags: Webfrontend
image: /assets/article_images/2015-01-25-webgl-shader/plexus.jpg
---


####html5 移动端 的resize 与 orientationchange

这一阵子又搞html5 的移动端video，发现很多有意思的问题，有一个问题很有趣

当我真机调试的时候，我特意观察了一下resize事件 和 orientationchange事件，结果发现，他们的调用顺序很有意思
safari上是 物理旋转 -> 触发 onresize -> 触发 onorientationchange 

所以我们不好在直接在旋转事件上修改内容，而在onresize上做文章是个比较好的选择
然而悲剧的是...你根本无法在这个环节判断orientation有木有变...

我想到的一种解决方案是：
通过将绑定动作放入一个闭包中，维护一个OldSize对象来保存 innerWidth 与 innerHeight,
每次都对这个对象进行判定看看是否有调换，
（这里需要注意，浏览器中的标题栏上浮下滑都会引起resize，所以可能不能直接===判定出来，而需要具体的行为根据各浏览器和系统的样式做区间设定，而像wechat webview这种较为固定的则更好判断一些，没错我就是说safari的标题栏和底栏...）


实际上对于样式来说实际上基本很难用到 onorientationchange 
基本上通过 clientWidth/Height 或者CSS的@media 都可有效解决

爬坑需要摸索，需要练习，否则在知识的汪洋大海中就会被水淹没，就会不知所措...
祝我身体健康，再见...

