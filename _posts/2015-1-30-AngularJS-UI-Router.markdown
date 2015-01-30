---
layout: post
title: AngularJS的UI Router 插件
date: 2015-01-30 13:55
categories: AngularJS UI-Router
tags: Webfrontend projects
image: /assets/article_images/angular/angular.jpg
---

##AngularJS的UI Router 插件##
> Angular UI Router时一个AngularJS的路由插件，你可以使用这个框架来组织你的页面
><br>
>与AngularJS的ngROuter使用url组织路由不同,UI-router使用的则是状态自动机

###开始使用###
1.安装

Bower：    ``` $ bower install angular-ui-router ``` <br>
npm:       ``` $ npm install angular-ui-router ``` <br>
Component: ``` $ Component install angular-ui/ui-router ``` <br>

2.引入文件

在页面引入　```<script src="js/angular-ui-router.js"></script>``` <br>

3.加入模块
```javascript
在模块里加入ui.router
var MyApp = angular.module('myApp', [
  'myApp.filters',
  'myApp.services',
  'myApp.directives',
  'myApp.controllers',
  'ngRoute',        //这里的ngRoute模块AngularJS自身的路由模块
  ‘ui.router’       //这是ui-router
]);
```

###使用ui-router###
未完待续=_=，直接翻译自github上的文档...

