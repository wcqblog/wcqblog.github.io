---
layout: post
title:  "angularjs的指令directive 1"
date:   2015-08-05 14:45:00
categories: Angular directive
tags: Webfrontend 
image: /assets/article_images/2015-01-25-webgl-shader/plexus.jpg
---
###有关angularjs的指令`directive` 1

angualarjs的指令是个可以很好的进行业务封装、模块化的工具

在我看来，他跟`SASS`中的`@mixin`有点像，或者说，一种`增强的宏`。

首先是可以封装业务代码

将一个`directive`看成是一个`class`的话
他有几个重要的参数

>restrict: 指令的声明风格 「A」代表`属性`, 「E」代表`元素`，「C」代表`样式`

那么我们用它封装模组件代码的时候，就是使用A；开发插件的话可以用E

>`template`/`templateURL`: 指明模板字符串或者模板路径

注意了，templateURL可以是一个`虚拟的地址`，它指向一个在现有的文件中指定id的innerHTML，通过

```html
	<script type='text/ng-template' id='myTmp.html'>
			<div>You Template HTML</div>
	</script>	
```		

另外，也可以通过使用`$http`或者其他的机制来加载模板，还在完成后再把模板内容设置给一个名为 `$templateCahce`的Angular对象。我们希望在指令运行之前模板已经位于缓存中，所以需要利用`module`上的`run`函数来调用模板
```javascript
	var app = angular.module('app',[]);
	app.run(function($templateCache){
		$templateCache.put('')
	});
```
这样能极大地减少请求数，在阅读`ui-botstrap.js`的源代码的时候，看到他就使用了这样的技术。

让我觉得很棒的一点是，他的这种写法，带来了清晰的分层，当用户在不同的决策进行迁移的时候，底层的改动不会引起上层应用代码的改动，真是帅呆了！

####`compile`方法和`link`方法
必须说，angular确实设计思想很好，为每个地方都提供的充分的扩展。

>1. 加载阶段：angular库正在页面加载的时候会查找ng-app指令从而找到应用的边界
>2. 编译阶段：angular遍历DOM结构，标识出模板中注册的所有指令。它会根据指令定义的规则来转换DOM结构美国存在`compile`函数，则调用它。调用`compile`函数将得到一个编译好的template函数，它将会调用从所有指令中搜集而来的link函数
>3. 连接阶段：为了让视图成为动态的，Angular会对每一条指令运行一个`link`函数。`link`函数的一般操作是在DOM或者模型上创建监听器，监听器会使视图和模型的内容随时保持同步

感觉很熟悉是吧
`compile`方法就像是这个`directive`的`static`方法

而`link`方法则是这个`class`的`prototype`，它是这个class`实例化对象`的方法，是针对具体的某一个scope的`listener`

`controller`方法不必多说，就是这个组件的控制器。

这种感觉，是不是声明式的？感觉很爽吧~~

###`scope`作用域

下节再说=_=我先搞点业务~~



