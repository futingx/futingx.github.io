---
layout: post
title:  "部署和使用makedown写jekyll"
categories: jekyll  # 文章类别
tags:  jekyll
author: FTX
---

* content
{:toc}

关于jekyll
建站前的碎碎念：
记得我了解jekyll还是在一次无意间刷小破站看到的，但是当时看到的并不是jekyll，是Hexo这个框架。为什么选择jekyll，因为在Comments看到一个blog评论了一句“jekyll不香吗？”
我当时就去搜索了一些相关jekyll的资料来查阅这个建站框架。一查惊艳到我了，jekyll的模型框架对于一个想要搭建一个小型的web或者一个分享知识的个人blog网站真是很不错。搭建好之后对于写文章也很友好，也有利于维护。具体关于jekyll可以自行官网了解：[jekyll](https://jekyllcn.com/docs/home/)
接下来我就简单具体介绍一下 jekyll 框架的用法：




  
#### 准备工作

##### 建站的必要工具
- ##### [ruby官网(下载最新的即可)](https://rubyinstaller.org/downloads/)
- #### jekyll（在ruby安装完成之后，在命令行操作）
&nbsp;
##### 安装ruby
1. 下载完存D盘（方便查找);
2. 点击安装ruby，根据提示依次完成安装，一般安装完 ruby 后会自动弹出一个新的命令行安装界面，我们需要在里面选择 3，然后回车，等待下载完成，过程可能会久一点，因为服务器在国外。如果最后还是安装无响应，可以百度解决;
3. 安装完成之后，输入以下命令安装 jekyll：
```
gem install jekyll bundler
```
完成这些下载，Jekyll 开发环境就搭建完成了。如果在安装过程中遇到任何安装问题，自行百度，会遇到各种各样问题，我第一次部署也是这样，不要气馁。&nbsp;
&nbsp;
#### 让你的jekyll网站在本地跑起来

&nbsp;首先，你要有一个自己的jekyll网站模板，没有的可以去[jekyll网站模板库](http://jekyllthemes.org/)淘一个自己喜欢的，注意认真阅读作者的readme文件，合理使用。

##### 在自己项目的 *根目录* 打开命令行安装依赖，接下来的操作都在命令行中。

---

&emsp;1.执行命令：`bundle install`如果这一步出错了大部分的报错一般情况都是你的版本和作者的不同导致，可以尝试一下`bundle update`这个命令让你的版本和作者的更新至一样的版本。

&emsp;2.通过 jekyll 服务把项目跑起来

&emsp;执行命令：`jekyll serve`如果这个命令报错可以重新尝试一下命令：`bundle exec jekyll serve`

&emsp;上述命令可以顺利执行完成，那么ruby和jekyll部署好了。
成功之后，项目默认打开在4000端口，可以浏览器https//localhost:4000访问你的项目。
&emsp;如果在这过程当中还存在错误无法在本地跑jekyll网站项目，可以参考这两篇文章重新检查错误：[建站指导01](https://blog.walterlv.com/post/setup-jekyll-in-windows.html)
&emsp;[建站指导02](https://jueee.github.io/2020/06/2020-06-17-%E6%90%AD%E5%BB%BAJekyll%E7%8E%AF%E5%A2%83/)