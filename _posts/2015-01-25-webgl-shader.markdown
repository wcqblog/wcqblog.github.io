---
layout: post
title:  "WebGL中的着色器 Shader"
date:   2015-01-25 18:40:00
categories: WebGL
tags: 前端 javascript webgl shader 着色器
image: /assets/article_images/2014-11-30-mediator_features/night-track.JPG
---
##WebGL中的着色器 Shader##

主要分两种
> 1. Vertext Shader 顶点着色器
> 2. Fragment Shader 片元着色器

不过webgl中的顶点着色器和片元着色器都是拼接的字符串。


##一个使用了着色器的webgl程序的结构##

<img src="/assets/article_images/2015-01-25-webgl-shader/2015-01-25_234943.jpg" alt="图1">
##initShader(gl, vshader, fshader)##

<img src="/assets/article_images/2015-01-25-webgl-shader/2015-01-25_235117.jpg" alt="图2">
<img src="/assets/article_images/2015-01-25-webgl-shader/2015-01-25_235138.jpg" alt="图3">

##顶点着色器内置类型和变量##

<img src="/assets/article_images/2015-01-25-webgl-shader/2015-01-25_235151.jpg" alt="图4">
##vec4数据类型##

<img src="/assets/article_images/2015-01-25-webgl-shader/2015-01-25_235207.jpg" alt="图5">

