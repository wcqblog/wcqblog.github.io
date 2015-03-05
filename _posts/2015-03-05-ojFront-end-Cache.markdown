---
layout: post
title:  "OJ前端的各种缓存使用"
date:   2015-3-5 16:06:00
categories: 前端 javascript 异步ajax 同步ajax localStorage OJ
tags: Webfrontend projects
image: /assets/article_images/dota/fxjj.jpg
---

##有关于OJ前端的各种缓存使用来简单的说一下好了。

主要有几种缓存：

  1. 模板的缓存。模板的缓存是直接放到localStorage中的，这里没有直接用hash而是用的版本号做控制（哈哈看没看出来我做了版本控制呢~），一个模板文件会存储为内容和版本号，后期看情况改版本号好不好吧。。。~
  因为维持业务的耦合性稍微低一些的缘故强行把依赖异步ajax+回调的过程改为了同步ajax+顺序执行，这样可以保证再某些特殊情况下的调用更方便（多见于数据模型和控制器的操作，回调函数中很难保存原来的上下文，需要依赖全局变量或者在回调函数中写出强耦合的语句，维护起来可能会带来一定困难）
  然后每一个model对应着一个template，当这个浏览器第一次加载当前版本的网站的时候会去ajax加载模板，之后该用户在这个浏览器下加载这个版本的网站内容时就只需要从localStorage中读取。另外由于模板文件并不会很大，所以每次从localStorage或者ajax加载完模板之后会存到对应的Model原型中，方便所有的Model类实例调用。另一方面juicer会对渲染过的原始模板进行缓存。改天研究研究这个是什么机制。。
    
    
  顺带着扯一点，就是ajax的async:false这一点。我时常在考虑一个问题就是这样的ajax什么情况下是好的，在什么样的情况下又是不好的？浏览器可以同时发送若干个请求，每个请求的响应情况不同，返回的时间点也有很大的差异。现在问题来了：如果我后请求的内容在逻辑上依赖先请求的内容，但是后请求的确先到达了，怎么办？所以异步ajax就要把请求2写在请求1的success方法中。好，那这样的话我连续请求5个这样的文件呢？显而易见，这将是一个非常深而且整体体积非常大的函数，而它的回调函数散落各处，你需要在层层函数的argument中来编写和调用。即便封装的很好，也需要创建很多副本来保存回调函数中可能要用到的上下文信息。异步ajax的意义在于IO不会阻塞js的其他操作，但是整体来看，绝大多数ajax情景下是js的操作基本都没有和ajax并行的情况，因为ajax提供内容，没有内容其他根本没有意义）。所以对于多个连续的ajax，除非文件很大，否则我还是偏向于同步的ajax,它可以让程序逻辑清晰易懂。
  
  2.其他文本内容。。。类似于浏览过的题目信息我也会做成数组存在ProblemModel的原型中，可以一定程度上减少加载次数。
  3.图片缓存。。。304吧，反正现在才用了一张。。。外链是他们的事了=_=。。。
  4.我真的好希望能够优化地极限一些~
  
  顺便贴一下丑陋的版本控制代码吧。。。
  ```javascript
  function checkTemplateUpdate(){
    ajax.send(
        {
            url: 'http://localhost:63342/github/ngtest/public/JSON/check_template_update.json',
            data: null,
            type: "GET",
            async:false,//阻塞,防止之后的操作出现无法读取版本号的问题
            dataType: "json",
            success: function(Data)
            {
                if(Data.status==1)
                {
                    window.templateVersionInfo = Data.versionInfo;
                    for( t in Data.versionInfo ){
                        var ver = localStorage.getItem(t+".Version");
                        if  (ver == null || ver != Data.versionInfo[t]){//版本不一致
                            localStorage.removeItem(t);
                        }
                    }
                }
                else
                {
                    topMessage({
                        Message:Data.error,
                        Type:'fail'
                    });
                }
            }
        }
    );
}
```

另一个函数，负责加载模板的。。。
```javascript
  //加载模板
Model.prototype.loadTemplate = function()
{
    var templateHTML = this.getTemplateText();
    var temp = this;
    if(templateHTML == null)
    {
        var templateInLocalStorage = localStorage.getItem(this.templatePath);
        if(templateInLocalStorage == null  ){//localStorage没有找到模板，则ajax请求
            ajax.send(
                {
                    url: this.getTemplateUrl(),
                    data: null,
                    type: 'GET',
                    async:false ,//阻塞异步
                    dataType: "html",
                    success: function(template)
                    {
                        templateHTML = template;
                        temp.template = template.replace(/[\r\n]/g,"");
                        localStorage.setItem(temp.templatePath,temp.template.toString());
                        localStorage.setItem(temp.templatePath+".Version",window.templateVersionInfo[temp.templatePath]);//版本号对齐，这里还是依赖了一个全局变量来传递。。。
                        return templateHTML;
                    }
                }
            );
        }
        else{
            this.template = templateInLocalStorage;
            return templateInLocalStorage;
        }

    }
    else
    {
        return templateHTML;
    }
};
```
