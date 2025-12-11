---
layout: default
title: Blog
description: Articles and notes by Rezka Leonandya.
permalink: /blog/
---
# Blog

<table class="listing">
  <tbody>
  {%- for post in site.posts -%}
    <tr>
      <td class="date">{{ post.date | date: "%b %e, %Y" }}</td>
      <td>
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      </td>
    </tr>
  {%- endfor -%}
  </tbody>
</table>
