---
title: "maze-dataset: Open-Source Maze Generation for ML Research"
excerpt: "Peer-reviewed Python package for generating, solving, and visualizing maze datasets — 80+ GitHub stars, used in neural-network reasoning research. Published in JOSS.<br/><img src='/images/maze-problem-solution.png' alt='A maze problem and its solution as images'>"
collection: portfolio
date: 2025-10-01
---

**Journal of Open Source Software, 2025** &middot; [Paper](https://doi.org/10.21105/joss.08633) &middot; [Code](https://github.com/understanding-search/maze-dataset)

_Joint work led by Michael Ivanitskiy, with Aaron Sandoval, Alexander F. Spies, Tilman Räuker, Cecilia Diniz-Behn, and Samy Wu Fung._

**TL;DR:** `maze-dataset` is an open-source Python library for generating maze datasets with algorithmic variety — different generation algorithms, difficulty parameters, and output representations (images, tokens, graphs) — with solvers and visualization built in. It has 80+ GitHub stars and is used by researchers studying neural-network reasoning and interpretability.

<img class="fig" src="/images/maze-problem-solution.png" alt="A maze problem and its solution encoded as images">

## Why it exists

Studying how neural networks reason requires training data whose difficulty and structure you can *control*. Mazes are ideal — classical algorithms provide ground truth at any scale — but every research group was rolling its own generation code. We built one configurable, tested, pip-installable library so datasets are reproducible across the community.

## My contribution

I co-developed the package and am a co-author on the peer-reviewed JOSS paper. The library powers the out-of-distribution evaluations in my [AAAI 2026 paper on logical extrapolation](/portfolio/maze-extrapolation/), which consumed its percolation and size parameters to generate controlled distribution shifts.

*Skills: Python packaging, open-source collaboration, API design, test-driven development, peer review.*
