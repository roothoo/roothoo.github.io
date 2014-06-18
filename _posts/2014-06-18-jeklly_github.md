---
layout: post
title:  "Jeklly Github"
date:   2014-06-18 15:25:02
categories: web 
tags: [jeklly, git]
---

## _config.yml设置

~~~
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

