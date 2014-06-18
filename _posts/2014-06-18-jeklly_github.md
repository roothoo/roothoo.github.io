---
layout: post
title:  "Jeklly Github"
date:   2014-06-18 15:25:02
categories: web 
tags: [jeklly, git]
---
**本文使用Jekyll Bootstrap模板**  

#### 安装Ruby,Jekyll
实际上，Ruby，Jekyll软件并非必须安装，这里安装的目的是为了方便本地调试  
按照[Github的Jekyll安装教程](https://help.github.com/articles/using-jekyll-with-pages)，这样安装软件版本与Github相同，避免兼容性问题  
  

#### 使用pygments 实现语法高亮  
参考资料：[Jekyll中的语法高亮](Pygments http://havee.me/internet/2013-08/support-pygments-in-jekyll.html)  
##### 配置
pygments的安装过程略去.  
实现语法高亮的关键是使用`pygmentize`生成pygments.css样式文件：  

```bash
#进入网站目录
$ pygmentize -S default -f html > your/path/pygments.css
```
然后将生成的样式文件添加到网站模板中，这里的模板文件为_includes/themes/twitter/default.html. 在`\<head\>`和`\</head\>`标签之间添加：  

```html
<link rel="stylesheet" href="{{ BASE_PATH }}/pygments.css">
```
##### 使用

```
```c
/* hello world demo */
#include <stdio.h>
int main(int argc, char **argv)
{
	    printf("Hello, World!\n");
		    return 0;
}
\\```
```

效果如下：

```c
/* hello world demo */
#include <stdio.h>
int main(int argc, char **argv)
{
	    printf("Hello, World!\n");
		    return 0;
}
```
  
  
#### _config.yml设置

~~~
#使用pygments 实现语法高亮
pygments: true

#markdown解释器设置为kramdown, 因为github对默认的Maruku支持不好
markdown: kramdown

#使用disqus评论
#设置disqus帐号(需要先在disqus注册)
short_name : roothoo 

#disqus默认有广告(dicovery box), 在 http://USERNAME.disqus.com/admin/settings/页面，
#将"discovery :show links to ...."去掉勾选,即可去除广告
comments :
provider : disqus
disqus : 
short_name : roothoo 
~~~

