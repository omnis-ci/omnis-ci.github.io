---
layout: page
title: Blog
---

{% for post in site.posts %}
# <a href="{{ post.permalink }}">{{ post.title }}</a>
{{ post.excerpt }}
{% endfor %}