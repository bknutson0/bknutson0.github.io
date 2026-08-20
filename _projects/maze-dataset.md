---
title: "maze-dataset: Open-Source Maze Generation for ML Research"
excerpt: "Peer-reviewed Python package for generating, solving, and visualizing maze datasets — 80+ GitHub stars, used in neural-network reasoning research. Published in JOSS."
image: /images/maze-formats-card.png
image_alt: "The same maze rendered as ASCII text, a pixel array, and a plot"
collection: projects
date: 2025-10-01
redirect_from:
  - /portfolio/maze-dataset/
---

**Journal of Open Source Software, 2025** &middot; [Paper](https://doi.org/10.21105/joss.08633) &middot; [Code](https://github.com/understanding-search/maze-dataset)

_Joint work led by Michael Ivanitskiy, with Aaron Sandoval, Alexander F. Spies, Tilman Räuker, Cecilia Diniz-Behn, and Samy Wu Fung._

**TL;DR:** `maze-dataset` is an open-source Python package for generating, solving, and visualizing mazes. This package offers a variety of maze generation and solving algorithms and flexible representation useful for visualization and machine learning. It has 80+ GitHub stars and is used by researchers studying neural-network reasoning and interpretability.

<img class="fig fig--wide" src="/images/maze-formats-card.png" alt="The same maze rendered as ASCII text, an RGB pixel array, and a plot">
<p class="fig-caption">One maze, three representations: ASCII text, pixel array, and plot. Figure from <a href="https://doi.org/10.21105/joss.08633">our JOSS paper</a>.</p>

## Why it exists

Studying how neural networks reason requires configurable data. Mazes are an excellent testbed because they are intuitive and easily visualized, and there are a variety of algorithms for both generating and solving them, allowing us to simulate distributional shifts while retaining access to ground truth solutions.

Before this package, existing maze datasets were often fixed or offered limited control over generation and representation. Many research groups seemed to be reinventing the wheel building their own datasets, but each was flawed in different ways. We built one configurable, tested, pip-installable library so datasets are reproducible across the community.

## My contribution

This work was spearheaded by my collaborator, [Michael Ivanitskiy](https://miv.name/). I'm a co-author on the JOSS paper, and the honest description of my role is _demanding downstream user_. The library powers the out-of-distribution evaluations in my [AAAI 2026 paper on logical extrapolation](/projects/maze-extrapolation/), and pushing it hard — generating tens of thousands of mazes across a variety of configurations — surfaced real bugs and design limitations. I fixed issues through pull requests and motivated several larger changes.

_Skills: Python, open-source contribution (issues, pull requests, code review), scientific software, peer review (JOSS)._
