---
layout: page
title: Blog
---

## System Evolution & Architecture Notes

{% for post in site.posts %}
### [{{ post.title }}]({{ post.url }})

*{{ post.date | date: "%B %d, %Y" }}*

{{ post.excerpt }}

---

{% endfor %}
