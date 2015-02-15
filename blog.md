---
layout: page
title: 
tagline: 
---
{% for post in site.posts limit:3 %}
<h2><a href="{{ BASE_PATH }}{{ post.url }}" >{{ post.title }}</a></h2>
<div class="date"><span>{{ post.date | date_to_string }}</span></div>
{{ post.content }}
<hr/>
{% endfor %}
#### Checkout [All Posts](/posts.html)
