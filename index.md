---
layout: page
title: 나의 AI 블로그
---

> 🤖 **n8n + Solar AI** 가 자동 생성하는 블로그 &nbsp;&nbsp;|&nbsp;&nbsp; 📝 총 **{{ site.posts | size }}**개 포스트

<div class="post-list" markdown="1">
{% for post in site.posts %}
### [{{ post.title }}]({{ post.url | relative_url }})
📅 {{ post.date | date: "%Y.%m.%d" }} &nbsp;·&nbsp; `{{ post.categories | first }}`

---
{% endfor %}
</div>
