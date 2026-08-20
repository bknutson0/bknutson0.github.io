---
title: "Projects"
permalink: /projects/
author_profile: true
redirect_from:
  - /portfolio/
---

{% assign sorted_projects = site.projects | sort: "date" | reverse %}
{% for project in sorted_projects %}
<div class="feature-card{% cycle '', ' feature-card--reverse' %}">
  <a href="{{ project.url }}"><img src="{{ project.image }}" alt="{{ project.image_alt }}"></a>
  <div>
    <h3><a href="{{ project.url }}">{{ project.title }}</a></h3>
    <p>{{ project.excerpt }}</p>
  </div>
  <p class="card-cue">Read the full story →</p>
</div>
{% endfor %}
