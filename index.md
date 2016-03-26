---
layout: page
title: MegaApuTurkUltra
tagline: Running a cool blog since yesterday!
---
{% include JB/setup %}

{% for post in site.posts limit:5 %}
  <h1><a href="{{ post.url }}">{{ post.title }}</a></h1>
  <p>{{ post.date | date: "%d %B %Y" }}</p>
  <div>{{ post.content}}</div>
  <a href="{{ post.url }}">Full post &amp; comments &rarr;</a>
  {% unless forloop.last %}<hr/>{% endunless %}
{% endfor %}

{% if site.posts.length > 5 %}
  <hr />
  
  <ul class="posts">
    {% for post in site.posts offset:5 %}	
      <h3><a href="{{ post.url }}">{{ post.title }}</a></h3>
      <p><small><strong>{{ post.date | date: "%B %e, %Y" }}</strong> | {{ post.category }}</small></p>			
    {% endfor %}	
  </ul>
{% endif %}