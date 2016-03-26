---
layout: page
title: MegaApuTurkUltra
tagline: Insert some cool tagline here
---
{% include JB/setup %}

<ul class="posts">
  {% for post in site.posts limit:5 %}
    <h1><a href="{{ post.url }}">{{ post.title }}</a></h1>
    <p>{{ post.date | date: "%d %B %Y" }}</p>
    <div>{{ post.content}}</div>
  {% endfor %}
  
  {% for post in site.posts %}	
    <h3><a href="{{ post.url }}">{{ post.title }}</a></h3>
    <p><small><strong>{{ post.date | date: "%B %e, %Y" }}</strong> | {{ post.category }}</small></p>			
  {% endfor %}	
</ul>