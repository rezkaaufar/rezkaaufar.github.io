---
layout: default
title: News
description: Updates from Rezka Leonandya.
permalink: /news/
---
# News

<table class="listing">
  <tbody>
  {%- assign news_items = site.news | sort: 'date' | reverse -%}
  {%- for item in news_items -%}
    <tr>
      <td class="date">{{ item.date | date: "%b %e, %Y" }}</td>
      <td><a href="{{ item.url | relative_url }}">{{ item.title }}</a>{% if item.description %}<div class="meta">{{ item.description }}</div>{% endif %}</td>
    </tr>
  {%- endfor -%}
  </tbody>
</table>
