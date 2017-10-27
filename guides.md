---
layout: page
title: Guides
---

{% assign sorted_guides = site.guides | sort: 'rank' %}
{% for guide in sorted_guides %}
# <a href="{{ guide.url }}">{{ guide.title }}</a>
{% include byline.html content=guide %}
{{ guide.summary }}
{% endfor %}