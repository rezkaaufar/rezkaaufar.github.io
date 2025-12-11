---
layout: default
title: Projects
description: Selected projects by Rezka Leonandya.
permalink: /projects/
---
# Projects

{%- assign projects = site.projects | sort: 'title' -%}
<ul>
  {%- for project in projects -%}
    <li>
      <a href="{{ project.url | relative_url }}">{{ project.title }}</a>
      {% if project.description %}<div class="meta">{{ project.description }}</div>{% endif %}
      {% if project.tags and project.tags.size > 0 %}
        <div class="meta">{% for tag in project.tags %}<span class="tag">{{ tag }}</span>{% endfor %}</div>
      {% endif %}
    </li>
  {%- endfor -%}
</ul>
