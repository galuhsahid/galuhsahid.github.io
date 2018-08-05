---
layout: grid-page
title: Sketches
meta: "Sketches by Galuh."
permalink: /sketches/
---

{% for item in site.data.sketches %}
<div class="panel"><img src="/images/sketches/{{ item.name }}.JPG" alt="{{ item.alt }}"></div>
{% endfor %}
