---
layout: default
title: Blog
description: Articles and notes by Rezka Leonandya.
permalink: /blog/
---
<a href="{{ '/' | relative_url }}" aria-label="Home">← Home</a>

# Blog

<div class="tag-filters">
  <button class="tag-btn active" data-tag="all">All</button>
  {%- assign all_tags = "" | split: "" -%}
  {%- for post in site.posts -%}
    {%- for tag in post.tags -%}
      {%- unless all_tags contains tag or tag == "" -%}
        {%- assign all_tags = all_tags | push: tag -%}
      {%- endunless -%}
    {%- endfor -%}
  {%- endfor -%}
  {%- assign all_tags = all_tags | sort -%}
  {%- for tag in all_tags -%}
    <button class="tag-btn" data-tag="{{ tag }}">{{ tag }}</button>
  {%- endfor -%}
</div>

<table class="listing">
  <tbody>
  {%- for post in site.posts -%}
    <tr data-tags="{{ post.tags | join: ',' }}">
      <td class="date">{{ post.date | date: "%b %e, %Y" }}</td>
      <td>
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      </td>
    </tr>
  {%- endfor -%}
  </tbody>
</table>

<script>
(function () {
  var buttons = document.querySelectorAll('.tag-btn');
  var rows = document.querySelectorAll('.listing tr[data-tags]');

  buttons.forEach(function (btn) {
    btn.addEventListener('click', function () {
      buttons.forEach(function (b) { b.classList.remove('active'); });
      btn.classList.add('active');

      var tag = btn.getAttribute('data-tag');
      rows.forEach(function (row) {
        if (tag === 'all') {
          row.style.display = '';
        } else {
          var tags = row.getAttribute('data-tags').split(',');
          row.style.display = tags.indexOf(tag) !== -1 ? '' : 'none';
        }
      });
    });
  });
})();
</script>
