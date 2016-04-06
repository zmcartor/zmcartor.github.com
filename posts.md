---
layout: page
title:
header: Pages
group: navigation
---
{% include JB/setup %}

<div class="row">

<h2> Most Recent Posts </h2>
  <div class="col-sm-6 col-md-4 col-lg-4">
      <div class="thumbnail">
      <img src="/images/{{ site.posts[0].image }}" alt="post image">
      <div class="caption">
       <a href="{{ site.posts[0].url }}"> <h3>{{ site.posts[0].title }}</h3> </a>
       <p> {{ site.posts[0].description }} </p>
      </div>
    </div>
  </div>

   <div class="col-sm-6 col-md-4 col-lg-4">
      <div class="thumbnail">
      <img src="/images/{{ site.posts[1].image }}" alt="post image">
      <div class="caption">
       <a href="{{ site.posts[1].url }}"> <h3>{{ site.posts[1].title }}</h3> </a>
       <p> {{ site.posts[1].description }} </p>
      </div>
    </div>
  </div>

 <div class="col-sm-6 col-md-4 col-lg-4">
      <div class="thumbnail">
      <img src="/images/{{ site.posts[2].image }}" alt="post image">
      <div class="caption">
       <a href="{{ site.posts[2].url }}"> <h3>{{ site.posts[2].title }}</h3> </a>
       <p> {{ site.posts[2].description }} </p>
      </div>
    </div>
  </div>

 </div> 

<h2> All Posts </h2>
<div class="row">
{% for post in site.posts offset:3 %}
  <div class="col-sm-6 col-md-4 col-lg-4">
      <div class="thumbnail">
      <img src="/images/{{ post.image }}" alt="post image">
      <div class="caption">
       <a href="{{ post.url }}"> <h3>{{ post.title }}</h3> </a>
       <p> {{ post.description }} </p>
      </div>
    </div>
  </div>
    {% endfor %}
</div>
