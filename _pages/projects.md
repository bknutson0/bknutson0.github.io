---
title: "Projects"
permalink: /projects/
author_profile: true
redirect_from:
  - /portfolio/
---

{% assign sorted_projects = site.projects | sort: "date" | reverse %}
{% for project in sorted_projects %}
{% assign rev = forloop.index0 | modulo: 2 %}
{% if rev == 1 %}{% include project-card.html project=project reverse=true %}{% else %}{% include project-card.html project=project %}{% endif %}
{% endfor %}
