---
layout: post
title:  "水水的时间轴插件~~"
date:   2015-03-18 10:20:00
categories: 前端 javascript html5 时间轴 插件
tags: Webfrontend projects
image: /assets/article_images/dota/fxjj.jpg
---
#水水的时间轴插件

##继续整理一下咱们的项目里面造的轮子吧~

###这是个时间轴插件，按照设计师的想法，比较复杂，根据鼠标移动会发生变化，纵向排列。一个个圆圈串起来的~
写起来还是蛮麻烦的，而且现在是没有动画版本的。
以后写个canvas版本的吧。

###上代码吧。。

>网页代码

```html
<div class="time-line">
    <div class="ul">
    </div>
    <script src="../js/time_line_controller.js"></script>
</div>
```

>javascript控制器 time_line_controller.js

```javascript
/**
 * Created by wungcq on 15/3/9.
 */
window.time_line_controller = function(){
    return this;
};
HTMLCollection.prototype._indexOf = function(elem){
    var index = 0;
    for(var i = 0; i<this.length; i++ ){
        if(this[i].innerHTML ==elem.innerHTML){
            return index;
        }
        else{
            index++;
        }
    }
    if(index>this.length){
        return -1;
    }
};
NodeList.prototype._indexOf = function(elem){
    var index = 0;
    for(var i = 0; i<this.length; i++ ){
        if(this[i].innerHTML ==elem.innerHTML){
            return index;
        }
        else{
            index++;
        }
    }
    if(index>this.length){
        return -1;
    }
};

time_line_controller.prototype.user_regist_year = 2011;
time_line_controller.prototype.today = new Date();

time_line_controller.prototype._init = function(callback){
    this.year = parseInt(this.today.getFullYear());
    this.month = parseInt(this.today.getMonth()) + 1;
    var fragment_str = "";
    for(var i = this.year; i >= this.user_regist_year; i--) {

        fragment_str += '<div class="year-point inactive"><div class="i"></div><time>'+i+'</time>';

        if(this.year == i) {
            for(var j = this.month; j>= 1; j--) {

                fragment_str+= '<div class="month-point-wrapper">'+'<div class="month-point-box"><div class="i month-point"></div></div>'+'<span class="hidden">'+j+'月'+'</span>'+'</div>';

            }
        }else {
            for(var k = 12; k>= 1; k--) {

                fragment_str+= '<div class="month-point-wrapper">'+'<div class="month-point-box"><div class="i month-point"></div></div>'+'<span class="hidden">'+k+'月'+'</span>'+'</div>';

            }
        }
        fragment_str += "</div>";
    }
    document.getElementsByClassName("time-line")[0].getElementsByClassName("ul")[0].innerHTML = fragment_str;
    this.bind();
};
time_line_controller.prototype.bind = function(){
    $(".year-point").mouseover(function(){
        var target = this;
        var year_points = target.parentNode.children;
        var index = year_points._indexOf(target);
        for(var i = 0; i< year_points.length;i++) {
            if(i!=index) {
                year_points[i].className = "year-point inactive";
            }
        }
        target.className = "year-point active";
        var month_number = target.children.length-2;
    });

    $(".month-point-wrapper").hover(function(){
        var elem = this;
        var points = elem.parentNode.getElementsByClassName("month-point-wrapper");
        var index = points._indexOf(elem)==-1 ? (-1):(points._indexOf(elem));
        //$(points[index].children[0].children[0]).css({"box-shadow": "0 0 1px 1px rgba(255, 255, 255, .6)"});
        if(index>-1){
            var r = 9;
            for(var i = index; i >= 0; i--) {
                if(r<1){
                    r=1;
                }
                $(points[i].getElementsByClassName("month-point")[0]).css({"width":r,"height":r});
                if(r == 1) {
                    $(points[i].getElementsByClassName("month-point")[0]).css({"box-shadow": "0 0 1px 1px rgba(255, 255, 255, .6)"});
                }
                r--;
            }
            r = 9;
            for(var j = index; j<points.length; j++) {
                if(r<1){
                    r=1;
                }
                $(points[j].getElementsByClassName("month-point")[0]).css({"width":r,"height":r});
                if(r == 1) {
                    $(points[j].getElementsByClassName("month-point")[0]).css({"box-shadow": "0 0 1px 1px rgba(255, 255, 255, .6)"});
                }
                r--;
            }
        }
    });
    $(".time-line").mouseleave(function(){
        $(".year-point").attr("class","year-point inactive");
    });
};

//实例化
window.time_line_controller_entity = new window.time_line_controller();
time_line_controller_entity._init();
```

>样式表 time_line.css

```css
.ul{

    text-align: left;
}
.time-line {
    display: block;
    position: fixed;
    left:calc(500px + 50%);
    top: 150px;
    margin: 0;
    width: auto;
    height: auto;
    padding-left: 5px;
    cursor: default;
}
.year-point {
    display: block;
    position: relative;
    width: 80px;
    height: auto;
    min-height: 20px;
    margin: 0;
    margin-top: 15px;
    padding-left: 10px;
    overflow: hidden;
}
.year-point>.i {
    display: inline-block;
    vertical-align: top;
    position: relative;
    border-radius: 100%;
    width: 10px;
    height: 10px;
    margin-bottom: 5px;
    background-color: #9e9e9e;
    border: 2px solid #6e6e6e;
    cursor: pointer;
}
.year-point .i:hover{
    background-color: #82a7d4;
}
.year-point time {
    display: inline-block;
    vertical-align: top;
    position: relative;
    margin-left: 10px;
    color: rgba(255, 255, 255, .8);
}
.year-point time:after {
    content: "年";
}
.year-point.active {
    height: auto;
    min-height: 20px;
}
.year-point.active .i {
    background-color: #82a7d4;
}
.year-point.inactive {
    height: 20px;
}
.month-point-wrapper {
    display: block;
    width:80px;
    /* margin: 10px; */
    /* margin-bottom: 15px; */
    position: relative;
    margin-left: -5px;
}
.month-point-wrapper span{
    display: inline-block;
    vertical-align: middle;
    color : rgba(255, 255, 255, .7);
    font-size: 10px;
}
.month-point-box {
    display: inline-block;
    vertical-align: top;
    text-align: center;
    position: relative;
    left: 0;
    width: 25px;
    height: 10px;
    padding-top: 5px;
    padding-bottom: 5px;
    line-height: 25px;
}
.year-point > a:first-child {
    margin-top: 5px;
}
.month-point {
    display: block;
    width: 9px;
    height: 9px;
    margin: auto;
    border-radius: 100%;
    background-color: #82a7d4;
    border: none;
    transition: all ease 0.1s;
    cursor: pointer;
}
.month-point-wrapper:hover{
    text-shadow: 0 0 1px rgba(255, 255, 255, .8);
}
.month-point:hover {
    box-shadow: 0 0 3px rgba(255, 255, 255, .6);
}
```

