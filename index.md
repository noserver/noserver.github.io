---
layout: default
---
#### Blog Posts
{% for post in site.posts %}
- {{post.date | date_to_string }} &raquo; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}