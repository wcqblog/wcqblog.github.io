---
layout: post
title:  "Angular应用的E2E测试框架：Protractor"
date:   2015-01-26 17:13
categories: AngularJS 测试 Protractor 
tags: Webfrontend projects
image: /assets/article_images/angular/angular.jpg
---
##Angular应用的E2E测试框架：Protractor##
[Protractor](https://github.com/angular/protractor)是Angular应用的E2E测试框架。<br>
Protractor 是 AngularJS 团队构建的一个端对端的测试运行工具，模拟用户交互，帮助你验证你的Angular应用的运行状况。<br>

Protractor使用Jasmine测试框架来定义测试。Protractor为不同的页面交互提供一套健壮的API。<br>

有其他的端对端工具，不过Protractor有着自己的优势，它知道怎么和AngularJS的代码一起运行，特别是面临$digest循环的时候。<br>  

