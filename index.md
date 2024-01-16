---
layout: default
title: Blog
---

# This is all written by ChatGPT. As of 2024 I have absolutely no experience in web development.

{% for post in site.posts %}
  - [{{ post.title }}]({{ post.url }})
{% endfor %}