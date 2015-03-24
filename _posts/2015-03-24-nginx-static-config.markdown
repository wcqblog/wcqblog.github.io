---
layout: post
title:  "关于一些nginx的静态文件配置"
date:   2015-03-24 15:30:00
categories: nginx OJ 
tags: Projects
image: /assets/article_images/2015-01-25-webgl-shader/plexus.jpg
---


##关于nginx配置静态文件与路由的一些配置内容。##

业务需求是这样的，无论响应怎样的请求路径，只要不是以/API开头的路径都会返回主页，如果发现路径有.*后缀那么就会判定为时Index.html所请求的资源。

OJ上用到的一些内容，当然是因为用了restify的缘故，API服务器嘛，所以根本不管模板渲染，之前不管是使用Ruby on Rails还是Express框架，都可以直接render或者redirect到某个页面，现在需要配置静态文件服务器了。

但是！restify的serveStatic真的是有点弱 搞了半天也不成功。

于是梁大大建议用nginx做吧。想想也不错

但是niginx接收的请求不能判断get/post，当初协定api统一用post请求，这下傻眼了。。临时把API全改名了，都用/api开头了
然后几番实验和修改，弄出来下面这份配置


``` shell
    server {
        listen 8080 default_server; 
        listen [::]:8080 default_server ipv6only=on;#监听8080端口
    
        root /home/user/FUCKOJ/public; #设置静态文件根目录
        index templates/index.html templates/index.htm; #设置index默认返回的路径
    
        server_name localhost; #设置服务器名
    
        location ~* ^.+\.(ico|gif|jpg|jpeg|png|html|htm)$ { #静态文件规则
            access_log   off;
            expires      30d;
        }
    
        location ~* ^.+\.(css|js|txt|xml|swf|wav)$ { #静态文件规则
            access_log   off;
            expires      24h;
        }
    
        location ~* ^/api/.*$ { #api请求规则
            proxy_pass  http://127.0.0.1:3000; #转发给应用服务器
        }
    
        location / { #网站根地址
            try_files $uri $uri/ /templates/index.html;
        }
    }
```
