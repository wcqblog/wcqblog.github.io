---
layout: post
title:  "AngularJS中的Controller"
date:   2015-01-27 09:17
categories: AngularJS JavaScript 
tags: Webfrontend
image: /assets/article_images/angular/angular.jpg
---
#AngularJS中的Controller

##几个注意点##
> 不要试图去复用Controller,一个控制器一般只负责控制一小块视图

> 不要在Controller中操作dom，这不是Controller的职责

> 不要在Controller中做数据格式化, ng有很好的表单控件

> 不要在Controller中做数据过滤操作，ng有filter服务

> 一般来说Controller不会互相调用，Controller的交互通过事件进行



```HTML
  <!DOCTYPE HTML>
<html lang="zh-cn" ng-app>
<head>
    <meta charset="UTF-8">
    <title>data-model</title>
    <style type="text/css">
        .ng-cloak {
            display: none;
        }
    </style>
</head>
<body class="ng-cloak">
<div ng-controller="MyController">
    你的名字： <input type="text" ng-model="username"/>
    <button ng-click="sayHello()">欢迎</button>
    <hr/>
    {{greeting}}
</div>
<script src="../angular-1.0.1.js" type="text/javascript"></script>
<script type="text/javascript">
    function MyController($scope) {
        $scope.username = "尊敬的客人，";
        $scope.sayHello = function() {
            $scope.greeting = "Hello~" + $scope.username + "!";
        };
    }
</script>
</body>
</html>
  
```
