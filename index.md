---
layout: page
title: 
tagline: 
---
{% for post in site.posts limit:3 %}
<h2><a href="{{ BASE_PATH }}{{ post.url }}" >{{ post.title }}</a></h2>
&raquo; <span><u>{{ post.date | date_to_string }}</u></span>
&raquo; {{ post.content }}
<hr/>
{% endfor %}
#### Checkout [All Posts](/posts.html)
