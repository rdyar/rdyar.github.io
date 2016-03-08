---
title: rdyar.github.io
description: A fortuitous concatenation of pixels
active: yes
---
{% for post in site.posts | limit:2 %}
   <h2><a href="{{post.url}}">{{post.title | upcase}}</a></h2>
   <p class="small">Posted on {{post.date | date: '%B %d, %Y'}}</p>
   <p>{{post.excerpt}}</p>
   <a href="{{post.url}}">Read More</a>
   <hr>
{% endfor %} 
