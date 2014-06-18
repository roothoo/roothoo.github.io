---
layout: post
title:  "建立GitHub Pages"
date:   2014-01-01 15:25:02
categories: git 
tags: [git, github-pages]
---

##建立GitHub Pages
1.  注册github帐号，在github.com新建一个代码仓库(repository)，名称为***username***.github.io，注意，***username***必须为你的github帐号的用户名.此处使用`roothoo.github.io`.

2.  安装git软件
在Debian系统下，输入git安装命令:`sudo apt-get install git`.  

配置github帐号：

~~~ 
git config --global user.name roothoo
git config --global user.email roothoo@gmail.com
~~~ 

生成ssh key文件`~/.ssh/id_rsa.pub`，并将其内容复制到github网站的[SSH Key](https://github.com/settings/ssh)中。

~~~ 
ssh-keygen -t rsa -C "roothoo@gmail.com"
ssh-add id_rsa
~~~ 

3.  建立GitHub Pages
clone代码仓库`roothoo.github.io`：

~~~ 
git clone git@github.com:roothoo/roothoo.github.io.git
~~~ 

进入文件夹`roothoo.github.io`，新建index.html文件，并上传到github:

~~~ 
echo "hello github_pages">index.html
git add index.html
git commit -m "index.html added"
git push origin master
~~~ 

等待数分钟后，登录你的git_pages网址：roothoo.github.io，即可看到index的内容.

(END)  

> Written with [StackEdit](https://stackedit.io/).
