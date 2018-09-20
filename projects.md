---
layout: page
title: Projects
permalink: /projects/index.html
---

Some of the things I've been working on recently.
<!-- {% assign my_projects = site.projects | sort: 'current', 'last' %} -->
<div class="posts">
  {% for project in site.projects %}
    <article class="post">

      <h2><a href="{{ project.url }}">{{ project.title }}</a></h2>

      <div class="entry">
        {{ project.excerpt }}
      </div>

      <a href="{{ project.url }}" class="read-more">More Details</a>
    </article>
  {% endfor %}
</div>