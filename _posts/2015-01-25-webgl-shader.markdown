---
layout: post
title:  "WebGL中的着色器 Shader"
date:   2015-01-25 18:40:00
categories: mediator feature
tags: webgl shader
image: /assets/article_images/2014-11-30-mediator_features/night-track.JPG
---
##WebGL中的着色器 Shader##

主要分两种
> 1. Vertext Shader 顶点着色器
> 2. Fragment Shader 片元着色器

不过webgl中的顶点着色器和片元着色器都是拼接的字符串。


##一个使用了着色器的webgl程序的结构##

| 阶段|  名称      |
|---:|:---|
|1    |顶点着色器  |
|2    |片元着色器  |
|3    |主程序JS代码|


##initShader(gl, vshader, fshader)##

| 参数  | 内容     |
|---:|:---|
|gl     |上下文    |
|vshader|顶点着色器|
|fshader|片元着色器|


| 返回值| 内容     |
|---:|:---|
|true   |初始化成功|
|false  |初始化失败|

##顶点着色器内置类型和变量##

|类型 |变量名      | 描述                                |
|---:|:---:|:---|
|vec4 |gl_Position |设定顶点位置（必须要有，否则报错）   |
|float|gl_PointSize|设定顶点大小（可以没有，默认1px大小）|


##vec4数据类型##

|float|float|float|float|
|---|---|---|---|

