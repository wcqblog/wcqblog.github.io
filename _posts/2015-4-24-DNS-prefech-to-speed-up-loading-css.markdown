---
layout: post
title:  "使用DNS预读取提高CSS加载速度"
date:   2015-04-24 10:37:00
categories: 前端 javascript 浏览器 
tags: Webfrontend 
image: /assets/article_images/2015-01-25-webgl-shader/plexus.jpg
---

##好久没更博客了
###这些天加班和项目还是好多。。。当然中间也积累了一些很有意思的技术和代码还有感悟，过两天真 搞完OJ就总结一下

刷微博看到的，先记下来。
[来源地址：伯乐头条](http://web.jobbole.com/82358/)

> DNS查找时间都很高， 如果能减少该时间并提速，便会让资源加载变得更加高效。幸运的是，有个很棒的技巧能让网站的加载时间变得更快。它被称为DNS预取，并且很容易实现。你所需做的是，在网页顶部添加以下属性作为链接(link)。

```html
<link rel="dns-prefetch" href="//host_name_to_prefetch.com">
```
