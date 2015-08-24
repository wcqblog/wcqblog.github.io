---
layout: post
title:  "angularjs的指令directive 2 动态插入指令"
date:   2015-08-06 23:38:00
categories: Angular directive $compile
tags: Webfrontend 
image: /assets/article_images/2015-01-25-webgl-shader/plexus.jpg
---

###Angularjs动态插入directive的不优雅方法之 $compile

对的，我想过很多遍，如果我要写一个tab页这种指令，我该怎么搞!
如果有很多动态插入的元素该怎么办？

查了查书，找到了解决方案，跟我想的差不多：`$compile`！

当然`$compile`也是服务，需要向controller中注入`$compile`才能使用。
angular 暴露了这个服务从而使模板不再是一次编译成型的静态模板，而是动态的~

```javascript
	//登录组件
app.directive('login', function () {
    return {
        restrict: 'E',
        transclude:true,
        scope:{},
        templateUrl:'templates/admin/user/login.html',
        controller: function ($scope) {
           //这里是组件的内容
        }
    }
});

//容器组件
app.directive('container', function () {
    return {
        restrict: 'E',
        transclude:true,
        scope:{},
        template:'<div>register<button ng-click="addLogin()">添加登录组件</button></div>',
        controller: function ($scope,$compile) {
           
            $scope.addLogin = function(){
                var login = '<login></login>';
                var $html = $compile(login)($scope);
                $('register').append($html);
            };
           
        }
    }
});
```

当然更好的方法是抽象公共容器控件，甚至是写一个服务来简化这种事情，不过最终的机制都是相通的，那就是编译指令，将其转化为html并插入到指定的文档位置中~


