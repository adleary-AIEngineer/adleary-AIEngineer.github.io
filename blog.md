---
layout: page
title: Notes From the Field
---

## System Evolution & Architecture Notes

{% for post in site.posts %}
### [{{ post.title }}]({{ post.url }})

*{{ post.date | date: "%B %d, %Y" }}*

{{ post.excerpt }}

---

{% endfor %}
