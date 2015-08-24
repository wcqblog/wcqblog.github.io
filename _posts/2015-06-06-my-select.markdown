---
layout: post
title:  "用CSS与js组合实现的平滑的dropdown select"
date:   2015-06-06 15:10:00
categories: 前端 CSS 
tags: Webfrontend 
image: /assets/article_images/2015-01-25-webgl-shader/plexus.jpg
---


从dropdown.js改的一部分，自己写的一部分，还没怎么封装。
跟botstrap配合使用效果更佳

```html
  <div class="form-group col-md-6">
    <label>状态</label>
    <div class="select" id="status-select">
      <input type="text" readonly="" class="fakeinput form-control" placeholder="状态">
      <ul>
        <li value="" selected="selected" class="option" tabindex="1">所有</li>
        <li value="0" tabindex="1" class="option">未发布</li>
        <li value="1" tabindex="1" class="option">已发布</li>
        <li value="2" tabindex="1" class="option">已撤回</li>
      </ul>
    </div>
  </div>
```

```css
  .select {
  position: relative;
  display: inline-block;
  vertical-align: top;
  margin-top: -10px;
}
.select * {
  box-sizing: border-box;
}
.select > input {
  display: block;
  position: absolute;
  width: 100%;
  text-overflow: ellipsis;
  top:0;
}
.select > input:focus ~ ul {
  transform: scale(1);
  -webkit-transform: scale(1);
  opacity: 1;
}
.select > ul {
  position: absolute;
  padding: 0;
  margin: 0;
  margin-left: 30px;
  left:0;
  right: 0;
  min-width: 150px;
  transform: scale(1,0);
  -webkit-transform: scale(1,0);
  opacity: 0;
  z-index: 10000;
  transition: transform ease .3s .1s,opacity ease-out .3s;
  -webkit-transition: -webkit-transform ease .3s .1s,opacity ease-out .3s ;
  -webkit-transform-origin: top;
  transform-origin: top;
}
.select > ul[placement=top-left] {
  transform-origin: bottom left;
  bottom: 0;
  left: 0;
}
.select > ul[placement=bottom-left] {
  transform-origin: top left;
  top: 0;
  left: 0;
}
.select > ul  > li {
  list-style: none;
  padding: 10px 20px;
}
.select > ul  > li.dropdownjs-add {
  padding: 0;
}
.select > ul  > li.dropdownjs-add > input {
  border: 0;
  padding: 10px 20px;
  width: 100%;
}

/* Theme */
.select > input[readonly] {
  cursor: pointer;
}
.select[data-dropdownjs][disabled] + .select > input[readonly] {
  cursor: default;
}
.select > ul {
  background: #FFF;
  box-shadow: 0 1px 6px rgba(0, 0, 0, 0.12), 0 1px 6px rgba(0, 0, 0, 0.12);
  padding: 10px;
  overflow: auto;
  max-width: 400px;
  top:40px;
}
.select > ul > li {
  cursor: pointer;
  word-wrap: break-word;
}
.select > ul > li.selected,
.select > ul > li:active {
  background-color: #eaeaea;
}
.select > ul > li:focus {
  outline: 0;
  outline: 1px solid #d4d4d4;
}

```

原生的js


```javascript
  
(function(){
    window.myDropDown = function(){
        this.url = window.location.href;
        return this;
    };

    window.myDropDown.prototype.init = function(){
        var selects = document.getElementsByClassName("select");
        var len = selects.length;


        for(var i = 0;i<len;i++) {
            var dropdown = selects[i];
            var selectedElement = dropdown.querSelector('.option[selected="selected"]') || dropdown.children[1].children[0];
            var input = dropdown.children[0];
            var val = selectedElement.getAttribute("value");
            var text = selectedElement.innerHTML;
            input.value = text;
            input.setAttribute("value",text);
            dropdown.setAttribute("value",val);
        }
        if(this.url.split('#')[1]=='auto'){
            document.getElementById("operator-input").value = 'chunqi.wcq';
            document.getElementById("status-select").setAttribute("value",'0');
            document.getElementById("status-select").getElementsByTagName("input")[0].value = '未发布';
            window.addEventListener('load',function(){
               var t = setTimeout(function(){
                    var d = $_$.import("d");
                   d.pushData();
               },500);
            });
        }
        this.bind();
    };
    window.myDropDown.prototype.bind = function(){
        document.getElementsByClassName("main")[0 ].addEventListener("click",function(e){
            var _target = e.target;
            if(_target.classList.contains("option")){
                var val = _target.getAttribute("value");
                var text = _target.innerHTML;
                var input = _target.parentNode.previousElementSibling;
                input.value = text;
                input.parentNode.setAttribute("value",val);
            }else {

            }
        },true);
    };
    window.dropdown_plugin = new myDropDown();
    window.dropdown_plugin.init();
})();

```
