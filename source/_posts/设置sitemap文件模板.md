---
title: 设置sitemap文件模板
date: 2019-01-11 17:41:07
tags: [搜索引擎,网站建设,sitemap]
categories: 学习
---

## 前言
&emsp;&emsp;今天尝试把博客加入谷歌搜索和百度搜索，网站所有权验证什么的都没什么问题，最后到添加sitemap的时候，百度那边添加完了没有任何提示，而谷歌这边显示sitemap文件有错误：“可以读取站点地图文件但网址无法访问”。检查sitemap文件发现里面的网址是这样的：`https://www.yoursite.com/about/index.html`。<!-- more -->
&emsp;&emsp;很明显这不对，应该吧`www.yoursite.com`换成自己的域名才对嘛。我使用的是Nexo的插件“hexo-generator-sitemap”和“hexo-generator-baidu-sitemap”来自动生成sitemap文件，生成sitemap文件的sitemap模板一直不知道怎么调整，也一直找不到文档，折腾了好几个小时才终于弄好。这篇博客记录一下修改模板的方法。

## 具体过程
* **第一步：安装插件**
百度的站点地图跟谷歌的不一样，所以有两个插件，分别用来生成百度和谷歌的站点地图文件
```
npm install hexo-generator-sitemap --save
npm install hexo-generator-baidu-sitemap --save
```
* **第二步：配置Hexo的_config.yml文件**
在文件的末尾添加如下内容：
```
# 自动生成sitemap
sitemap:
    path: sitemap.xml
baidusitemap:
    path: baidusitemap.xml
```
* **第三步：修改sitemap文件模板：**
    1. 谷歌的：修改“node_modules\hexo-generator-sitemap\sitemap.xml”文件第5行
        * 默认的是这样的：
        ```
            <loc>{{ post.permalink | uriencode }}</loc>
        ```
        * 修改后是这样的：
        ```
            # 把域名替换成自己的即可
            <loc>https://www.xiewenqi.com/{{ post.path }}</loc>
        ```
    2. 百度的：修改“node_modules\hexo-generator-baidu-sitemap\baidusitemap.ejs”文件第14行
        * 默认的是这样的：
        ```
        <loc><%- encodeURI(url + post.path) %></loc>
        ```
        * 修改后是这样的
        ```
        # 把域名替换成自己的即可
        <loc><%- encodeURI("https://www.xiewenqi.com/" + post.path) %></loc>
        ```
* **第四步：生成sitemap文件：**
执行Hexo命令：`hexo g`，然后就可以在博客文件夹的“public”文件夹下看到两个新生成的sitemap文件了。