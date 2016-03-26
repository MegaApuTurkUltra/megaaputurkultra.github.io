---
layout: page
title: MegaApuTurkUltra
tagline: Insert some cool tagline here
---
{% include JB/setup %}

<ul class="posts">
  {% for post in site.posts limit:5 %}
    <h3><a href="{{ post.url }}">{{ post.title }}</a></h3>
    <p>{{ post.date | date: "%d %B %Y" }}</p>
    <div>{{ post.content}}</div>
  {% endfor %}
</ul>