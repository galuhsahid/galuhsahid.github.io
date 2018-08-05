---
layout: page
title: Projects
meta: "Description, code and demo of Galuh's projects which range from web development to data science."
permalink: /projects/
---

<ul class="list">
{% for item in site.data.projects %}
    <li class="list__item">
        <div class="list__title">{{ item.name }}</div>
        <div class="list__description">{{ item.description }}</div>
        <div class="list__details">
            {% if item.code %}
                <div class="list__sub-item"><span class="list__code"><a href="{{ item.code }}">Code</a></span></div>
            {% endif %}

            {% if item.demo %}
                <div class="list__sub-item"><span class="list__code"><a href="{{ item.demo }}">Demo</a></span></div>
            {% endif %}
        </div>
        <div class="list__tags">
            <div class="list__sub-item">
                <span class="list__category">{{ item.category }}</span>

                {% for stack in item.stack %}
                    <span class="list__stack">{{ stack.name }}</span>
                {% endfor %}
            </div>
        </div> 
    </li>
{% endfor %}
</ul>

