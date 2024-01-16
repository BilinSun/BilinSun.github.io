---
layout: default
title: Blog
---

{% for post in site._posts %}
  - [{{ post.title }}]({{ post.url }})
{% endfor %}