---
layout: welcome
title: XiaLuXing
---
{% include JB/setup %}

#### 用GIT来管理blog

用textile写文档还是很顺手的，现在在加上GIT 估计我就能坚持写blog了。

<hr>
####  最近的日志

<ul class="posts">
  {% for post in site.posts limit:10  %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
