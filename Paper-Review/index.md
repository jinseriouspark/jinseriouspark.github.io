---
layout: default
title: "Paper Review"
description: 광고 논문을 읽고 정리합니다
main: true
project-header: true
#header-img: 
---

<ul class="catalogue">
{% assign sorted = site.pages | sort: 'order' | reverse %}
{% for page in sorted %}
{% if page.paper-review == true %}
{% include post-list.html %}
{% endif %}
{% endfor %}
</ul>
