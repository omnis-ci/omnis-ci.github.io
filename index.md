---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: home
---
# Guides
{% assign sorted_guides = site.guides | sort: 'rank' %}
{% for guide in sorted_guides %}
* [{{ guide.title }}]({{ guide.url }})
{% endfor %}

# Blog
{% for post in site.posts %}
* [{{ post.title }}]({{ post.url }})
{% endfor %}
