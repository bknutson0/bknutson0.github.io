---
permalink: /
title: "About me"
author_profile: true
redirect_from:
  - /about/
  - /about.html
---

I'm a PhD candidate in [Applied Mathematics and Statistics](https://ams.mines.edu/) at the Colorado School of Mines, advised by [Dr. Daniel McKenzie](https://danielmckenzie.github.io/) and [Dr. Samy Wu Fung](https://swufung.github.io/). My research lies at the intersection of deep learning and optimization: I study neural networks that can think longer to solve harder problems, and how to train them faster. **I'm seeking data science and machine learning roles starting early 2027.**
{: .lead}

[Resume](/files/resume.pdf) · [CV](/files/CV.pdf)

## What I do

<div class="skill-card">
  <i class="fas fa-screwdriver-wrench" aria-hidden="true"></i>
  <div>
    <h3>Build</h3>
    <p>I love to design the simplest system that solves the problem, adding complexity only when necessary. At <strong>Oak Ridge National Laboratory (ORNL)</strong>, I helped build a geospatial ML pipeline covering 898,000 buildings. For my maze research, I co-authored <a href="https://github.com/understanding-search/maze-dataset">maze-dataset</a>, a peer-reviewed open-source package with <strong>80+ GitHub stars</strong>. Across projects, I work in Python, PyTorch, SQL, Docker, and AWS/GCP.</p>
  </div>
</div>

<div class="skill-card">
  <i class="fas fa-magnifying-glass-chart" aria-hidden="true"></i>
  <div>
    <h3>Evaluate</h3>
    <p>I stress-test models to find where they break and why, because you don't know a system until you know its limits. <strong>Our AAAI 2026 paper</strong> traced neural networks' out-of-distribution failures to a hidden shortcut heuristic, and my controlled ablation at <strong>ORNL</strong> showed that a recent global height dataset adds nothing to prediction accuracy.</p>
  </div>
</div>

<div class="skill-card">
  <i class="fas fa-comments" aria-hidden="true"></i>
  <div>
    <h3>Communicate</h3>
    <p>I explain complex ideas simply because it forces me to understand deeply. I've briefed multi-billion-dollar cost analyses to senior decision-makers across federal agencies including the <strong>US Navy, Air Force, and NASA</strong>, taught Differential Equations to 60 students as Instructor of Record, and led a coding bootcamp for incoming PhD students.</p>
  </div>
</div>

## Featured projects

{% assign featured = "maze-extrapolation,maze-dataset,ornl-building-heights" | split: "," %}
{% for slug in featured %}
{% capture project_url %}/projects/{{ slug }}/{% endcapture %}
{% assign project = site.projects | where: "url", project_url | first %}
{% assign rev = forloop.index0 | modulo: 2 %}
{% if rev == 1 %}{% include project-card.html project=project reverse=true %}{% else %}{% include project-card.html project=project %}{% endif %}
{% endfor %}

## Off the clock

Outside of work, you'll find me on a pickleball court, hiking Colorado trails, gaming, or listening to Noah Kahan.
