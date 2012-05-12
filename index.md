---
layout: welcome
title: XiaLuXing
---
{% include JB/setup %}

[侠路行](http://xialuxing.com/)这个域名买到10年后了,然后在将总的博客上[发现](http://www.worldhello.net/2011/11/29/jekyll-based-blog-setup.html)可以用Git来管理博客,太好了,这回估计我会坚持写下去.不怕一段时间没打理就消失不见.

<hr>
####  最近的日志

<ul class="posts">
  {% for post in site.posts limit:10  %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}/">{{ post.title }}</a></li>
  {% endfor %}
</ul>
