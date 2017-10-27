---
layout: page
title: Blog
---

{% for post in site.posts %}
# <a href="{{ post.url }}">{{ post.title }}</a>
{{ post.excerpt }}
{% endfor %}