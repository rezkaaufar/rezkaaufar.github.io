---
layout: default
title: Rezka Leonandya
description: "Rezka Leonandya's personal site and blog about data science, NLP, and engineering."
---
# Hi, I'm Rezka.

End-to-end Machine Learning Engineer and Data Scientist (Maps) working at <a href="https://www.gojek.io/" target="_blank" rel="noreferrer">Gojek</a>. 

MSc in Artificial Intelligence from the University of Amsterdam.

<ul class="social-links">
  <li><a href="mailto:rezka.aufar@gmail.com"><span class="icon">✉️</span>Email</a></li>
  <li><a href="https://github.com/rezkaaufar" target="_blank" rel="noreferrer"><span class="icon">🐙</span>GitHub</a></li>
  <li><a href="https://www.linkedin.com/in/aufarleo" target="_blank" rel="noreferrer"><span class="icon">🔗</span>LinkedIn</a></li>
  <li><a href="https://leetcode.com/u/rezka/" target="_blank" rel="noreferrer"><span class="icon">🧩</span>LeetCode</a></li>
</ul>

## Latest writing
<table class="listing">
  <tbody>
  {%- for post in site.posts limit:5 -%}
    <tr>
      <td class="date">{{ post.date | date: "%b %e, %Y" }}</td>
      <td>
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      </td>
    </tr>
  {%- endfor -%}
  </tbody>
</table>
<p class="meta"><a href="{{ '/blog/' | relative_url }}">Browse all posts</a></p>

## Publications
<p>See my full list on <a href="https://www.semanticscholar.org/author/Rezka-Leonandya/9290871" target="_blank" rel="noreferrer">Semantic Scholar</a>.</p>

## Resume
<p><a href="/assets/pdf/resume.pdf" target="_blank" rel="noreferrer">View my resume (PDF)</a></p>
