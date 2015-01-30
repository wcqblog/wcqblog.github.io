---
layout: post
title:  "AngularJS的文件资源地址以及１.3.x中Angularjs初始化报错的问题"
date:   2015-01-30 13:55
categories: AngularJS 资源　Failed_to_instantiate_module_[$injector:unpr]
tags: Webfrontend projects
image: /assets/article_images/angular/angular.jpg
---
<br>
###AngularJS的文件资源地址###
<br>
目前AngularJS的文件资源不是特别好找，找到一个不错的网址
> https://code.angularjs.org/version/
>>version是所用版本号<br>
>>我的是1.3.11,所以是 https://code.angularjs.org/1.3.11/
[AngularJS files Download-link](https://code.angularjs.org/1.3.11/)

<br>

###问题解决:Failed to instantiate module [$injector:unpr] Unknown provider: $routeProvider###
在stackoverflow上面查到的,主要原因是,在AngularJS 1.2之后route模块已经不在angularJS的核心模块了,所以需要加载Angular-route.js，并且在模块初始化函数种添加ngRoute模块
<br>
```
  <script src="lib/angular/angular-route.min.js"></script>  
```

```javascript
'use strict';
// Declare app level module which depends on filters, and services
angular.module('myApp', [
  'myApp.filters',
  'myApp.services',
  'myApp.directives',
  'myApp.controllers',
  'ngRoute'//这里的ngRoute模块是后添加上去的路由模块
]).
config([
  '$routeProvider',
  function($routeProvider) {
      $routeProvider.when('/view1', {
        templateUrl: 'partials/partial1.html',
        controller: 'MyCtrl1'
        });
      $routeProvider.when('/view2',{
        templateUrl: 'partials/partial2.html',
        controller: 'MyCtrl2'
      });
      $routeProvider.otherwise({
        redirectTo: '/view1'
      });
    }]);
```

另一种解决方法则是,使用yeoman来安装依赖...记得应该有...
日后多多补充～～
