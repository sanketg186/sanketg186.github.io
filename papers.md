---
layout: page
title: Papers
permalink: /papers/
---

# Research Papers & Explanations

This section contains detailed explanations and analyses of interesting research papers in machine learning, AI, and related fields.

---

{% for paper in site.papers %}
### [{{ paper.title }}]({{ paper.url | relative_url }})
**Authors:** {{ paper.authors }}  
**Year:** {{ paper.year }}  
**Venue:** {{ paper.venue }}  
**Categories:** {{ paper.categories | join: ", " }}

{{ paper.excerpt | strip_html | truncatewords: 50 }}

**Key Contributions:**
- Introduces BPE as a data-driven approach to subword segmentation
- Shows substantial improvements in translation quality for rare words
- Demonstrates the effectiveness of subword units in neural translation models

*Explained on {{ paper.date | date: "%B %d, %Y" }}*

---
{% endfor %}

*This page is regularly updated with new paper explanations and analyses.* 