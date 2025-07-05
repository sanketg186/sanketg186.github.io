---
layout: page
title: Papers
permalink: /papers/
---

# Research Papers & Explanations

This section contains detailed explanations and analyses of interesting research papers in machine learning, AI, and related fields.

---

<div class="posts">
  {% for paper in site.papers reversed %}
    <article class="post">
      <h1><a href="{{ site.baseurl }}{{ paper.url }}">{{ paper.title }}</a></h1>
      
      <div class="paper-meta">
        <p><strong>Authors:</strong> {{ paper.authors }}</p>
        <p><strong>Year:</strong> {{ paper.year }} | <strong>Venue:</strong> {{ paper.venue }}</p>
        <p class="paper-categories">
          <strong>Categories:</strong>
          {% for category in paper.categories %}
            <span class="category-tag">{{ category }}</span>
          {% endfor %}
        </p>
      </div>

      <div class="entry">
        {{ paper.excerpt }}
      </div>

      <div class="paper-date">
        <!-- <em>Explained on {{ paper.date | date: "%B %e, %Y" }}</em> -->
      </div>

      <a href="{{ site.baseurl }}{{ paper.url }}" class="read-more">READ MORE</a>
    </article>
  {% endfor %}
</div>
