---
title: "Can Neural Networks Think Longer to Solve Harder Problems?"
excerpt: "Stress-testing test-time compute: I evaluated recurrent and implicit networks on out-of-distribution maze-solving, uncovered a hidden heuristic behind their success, and quantified a train-data diversity trade-off. Published at AAAI 2026.<br/><img src='/images/maze-generalization.png' alt='Models trained on easy mazes are tested on much harder ones'>"
collection: portfolio
date: 2026-01-22
---

**Published at AAAI 2026** &middot; [Paper](https://arxiv.org/abs/2410.03020) &middot; [Code](https://github.com/mines-opt-ml/maze-extrapolation) &middot; [Talk video](https://underline.io/lecture/139985-on-logical-extrapolation-for-mazes-with-recurrent-and-implicit-networks) &middot; [Slides](/files/aaai26-slides.pdf) &middot; [Poster](/files/aaai26-poster.pdf) &middot; [maze-dataset package](https://github.com/understanding-search/maze-dataset)

**TL;DR:** Some neural networks can "think longer" at test time by iterating more. I stress-tested whether that extra compute lets them solve problems *harder* than anything they saw in training. It does — but only along certain axes: the models quietly learned a shortcut heuristic instead of a general algorithm, and diversifying the training data fixed some failures while creating others.

## The problem

Production models constantly face inputs harder than their training data. Recurrent networks (RNNs) and implicit networks (INNs) offer a tempting fix: their depth is adjustable at test time, so they can spend more compute on harder inputs — like a person thinking longer about a harder puzzle. I investigated when this actually works and how it fails.

<img src="/images/maze-generalization.png" alt="Train on small, easy mazes; test on large mazes and mazes with loops" width="70%">

Maze-solving is the ideal testbed: classical algorithms provide ground-truth solutions at any difficulty, so I could train on small, easy mazes (9×9, no loops) and generate unlimited *harder* test mazes along controlled dimensions — bigger grids, "percolated" mazes containing loops, and shifted start positions.

## What I did

- Evaluated a pre-trained RNN and a custom-trained INN (PyTorch) across thousands of out-of-distribution mazes, sweeping test difficulty along size and percolation simultaneously.
- Compared predictions against classical maze algorithms to identify *what the models actually learned*, not just their accuracy.
- Used topological data analysis (Betti numbers via persistent homology) to rigorously characterize what the models' iterates converge to.
- Retrained both architectures across a range of training-data diversity levels, against a 10×-larger feedforward baseline.

## What I found

**1. Test-time compute works — dramatically.** With enough iterations, the RNN solves mazes 10× larger than anything in training at near-perfect accuracy, and both iterative models beat the feedforward baseline with 10× more parameters.

<img src="/images/maze-extrapolation.png" alt="Accuracy vs. maze size: more test-time iterations extend near-perfect accuracy to much larger mazes">

**2. But the model learned a heuristic, not the goal.** The RNN's predictions match *dead-end filling*, a classic algorithm that provably fails on mazes with loops — and indeed the RNN collapses on them. This is goal misgeneralization: perfect training accuracy concealed that the model never learned to "solve mazes," only a shortcut that happened to work on loop-free training data.

<img src="/images/maze-rnn-loop-failure.png" alt="On a maze with loops, the RNN's prediction retains loops instead of a valid path" width="70%">

**3. Data diversity is a trade-off, not a free lunch.** Adding just 0.3% wall-removal to training mazes unlocked loop-solving. But as diversity increases, accuracy concentrates around the training distribution and size extrapolation *degrades* — a concrete, quantified case of a data-curation trade-off.

<img src="/images/maze-combined-heatmaps.png" alt="Test accuracy heatmaps: diversifying training data helps loops but hurts size extrapolation">

**4. A standing theoretical assumption didn't survive contact with data.** Prior work assumed iterates must converge to a fixed point for extrapolation. Topological analysis revealed the RNN often converges to cycles instead — while still achieving perfect accuracy.

## Why it matters beyond mazes

Every finding is a production-ML lesson in miniature: in-distribution accuracy can hide the fact that a model learned the wrong thing; evaluation must probe *multiple* axes of distribution shift; and "add more diverse data" reallocates capability rather than uniformly adding it. The evaluation framework — controlled difficulty dimensions with algorithmic ground truth — is the same discipline behind any serious model-validation pipeline.

*Skills: PyTorch, large-scale experiment design, OOD evaluation, model debugging & failure analysis, topological data analysis, scientific communication (peer-reviewed AAAI paper + conference talk).*
