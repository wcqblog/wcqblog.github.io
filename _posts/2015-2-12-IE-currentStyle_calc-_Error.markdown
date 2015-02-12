---
layout: post
title:  "CSS calc() 以及IE的currentStyle的对百分比CSS解析问题"
date:   2015-2-12 13:30:00
categories: 前端 javascript 浏览器 耿直的IE  _css() calc
tags: Webfrontend 
image: /assets/article_images/2015-01-25-webgl-shader/plexus.jpg
---
###表示不太好忍###
calc()函数是CSS3比较让人喜欢的一个地方。
calc计算一个表达式，并且能够动态的计算。
以前我如果写一个盒子，他距离浏览器四个边各90px,做的事情是需要这样的样式

```CSS
#wrapper{
}
#box {
  width:auto;
  height:auto;
  margin:90px;
}
```
以及这样的js

```javascript
function set_style(){
  var wrapper = $("#wrapper");
  var box = $("#box");
  box.css({
    width:function() {
      return (wrapper.width-180);
    },
    height:function() {
      return (wrapper.height-180);
    }
  });
}
//你需要不断地去检测wrapper的大小变化
$("#wrapper").resize(function(){
  set_style();
});
  
```
你会发现，明明这就是CSS在干的事情，凭什么要我js去写啊。。。
so。。CSS3加入了calc()
上面这一坨代码就变成了

```css
  #box{
    width: calc(100% - 180px);
    height: calc(100% - 180px);
    margin: 90px;
  }
```
不过比较难过的是:IE9以下版本的IE不支持docment.defaultView.getComputedStyle，而IE自己的currentStyle都特别耿直，百分比的样式只是取出来，并不管计算，所以常用的百分比样式需要你取到其父元素的innerHeight/innerWdith等属性再乘以你取出的百分比(用js取，不是用css)，这里是需要特别注意的。

W3C真的是码农的救星啊
不过天朝将近10%的IE8占有率真是虐心。。世界平均才1%多一点...光这些就能够气死中国的开发者了。心塞。
####你问我有森么建议####
不要再去兼容了。。。

优雅降级虐哭我=_=。
