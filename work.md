---
layout: page
title: Work
permalink: /work/
---

<div class="posts">
  {% for work in site.works %}
    <article class="post">

      <div class="entry">
        {{ work.content }}
      </div>
      {{ work.content.length }}
    </article>
  {% endfor %}
</div>
