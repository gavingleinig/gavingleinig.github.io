---
title: "Projects"
permalink: /projects/
layout: single
author_profile: true
---

Three graduate research/coursework projects from my MS in Computer Science at Texas A&M.

{% assign sorted = site.portfolio | sort: "date" | reverse %}
<div class="grid__wrapper">
  {% for post in sorted %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>
