---
layout: post
title:  "原生js对页面元素的样式操作，以及一个封装好的_css()函数"
date:   2015-2-4 13:30:00
categories: 前端 javascript 浏览器 Node.prototype扩展 _css()
tags: Webfrontend 
image: /assets/article_images/2015-01-25-webgl-shader/plexus.jpg
---

##原生js对页面元素的样式操作，以及一个封装好的_css()函数##
当大家都习惯了用封装好的jquery来操作css的时候，有木有想过原生js获取样式的代码呢？

来取一下elem元素的attr样式，
> 注意，这里的elem是node，即document.getElem***()返回的节点

这里还是要区分浏览器内核的
### ie注定天生与众不同 ###
不过IE10以上对于自身的和标准写法都支持了

```javascript
  //说实话IE在很多方面做得还是很好的，比如人家IE的border-box盒模型。。再比如这个获取元素属性的方法
  //注意，IE的要求是驼峰书写。例如 margin-top --> marginTop
  //首先是行内样式elem.style
  var attrStyleOfElem = elem.style[attr];
  //然后是计算出来的样式
  var attrStyleOfElem = elem.currentStyle[attr];
```

### 再看看W3C标准 ###

```javascript
  //attr这里不是驼峰的了！ margin-top 这样就OK
  var attrStyleOfElem = document.defaultView.getComputedStyle(elem,null).getPropertyValue(attr);
  //我去好长好长。。。
```

### 再来说一下设置样式 ####

```javascript

//最终这些样式就是标签内的 style属性的值！
  var attrString = "margin-top: 20px; padding: 10px 0;"
  elem.style.cssText = attrString;
  
```

###最后送上一点福利，自己手动扩展了一下Node对象，支持elem._css()获取和设置样式###
###而且还支持链式写法哦~###

```javascript
//扩展 css
//有时间再扩展一下对象数组的写法
Node.prototype._css = function(attr,value){
    if(value!=null && typeof value!="undefined" ){
        if(typeof value!="")
        this.style[attr]=value;

        return this;//返回本对象，方便链式调用~
    }
    else{
        if(this.style[attr]){
            //若样式存在于html中,优先获取
            return this.style[attr];
        }else if(this.currentStyle){
            //IE下获取CSS属性最终样式(同于CSS优先级)
            return this.currentStyle[attr];
        }else if(document.defaultView && document.defaultView.getComputedStyle){
            //W3C标准方法获取CSS属性最终样式(同于CSS优先级)
            //注意,此法属性原格式(text-align)获取的,故要转换一下
            attr=attr.replace(/([A-Z])/g,'-$1').toLowerCase();
            //获取样式对象并获取属性值
            return document.defaultView.getComputedStyle(this,null).getPropertyValue(attr);
        }else{
            return null;
        }
    }

};
//示例
//获取width属性
var elem =  document.getElementsById('elem');
var width = elem.css("width");
设置elem元素的高度和背景颜色
elem.css("height",width).css("background-color","#369");
```

欢迎补充~
