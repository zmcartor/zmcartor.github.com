---
layout: page
title: All Posts
header: Pages
group: navigation
---
{% include JB/setup %}

<ul class="posts">
  {% for post in site.posts %}
    <li class="post-list"><span class="date">{{ post.date | date_to_string }}</span> <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

 ![](/images/smashedcomputers.jpg)
