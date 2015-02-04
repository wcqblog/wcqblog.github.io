---
layout: post
title:  "原生js对页面元素的样式操作"
date:   2015-02-04 13:30:00
categories: 前端 javascript 浏览器 
tags: Webfrontend 
image: /assets/article_images/2015-01-25-webgl-shader/plexus.jpg
---

##原生js对页面元素的样式操作##
当大家都习惯了用封装好的jquery来操作css的时候，有木有想过原生js获取样式的代码呢？

来取一下elem元素的attr样式，
> 注意，这里的elem是node，即document.getElem***()返回的节点

这里还是要区分浏览器内核的
### ie注定天生与众不同
不过IE10以上对于自身的和标准写法都支持了

```javascript
  //说实话IE在很多方面做得还是很好的，比如人家IE的border-box盒模型。。再比如这个获取元素属性的方法
  //注意，IE的要求是驼峰书写。例如 margin-top --> marginTop
  //首先是行内样式elem.style
  var attrStyleOfElem = elem.style[attr];
  //然后是计算出来的样式
  var attrStyleOfElem = elem.currentStyle[attr];
```

### 再看看W3C标准

```javascript
  //attr这里不是驼峰的了！ margin-top 这样就OK
  var attrStyleOfElem = document.defaultView.getComputedStyle(elem,null).getPropertyValue(attr);
  //我去好长好长。。。
```

### 再来说一下设置样式 


```javascrpit
  var attrString = "margin-top: 20px; padding: 10px 0;"
  elem.style.cssText = attrString;
  //最终这些样式就是标签内的 style属性的值！
```
