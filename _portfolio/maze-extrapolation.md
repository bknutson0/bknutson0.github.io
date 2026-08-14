---
title: "Can Neural Networks Think Longer to Solve Harder Problems?"
excerpt: "Stress-testing test-time compute: I evaluated recurrent and implicit networks on out-of-distribution maze-solving, uncovered a hidden heuristic behind their success, and quantified a train-data diversity trade-off. Published at AAAI 2026.<br/><img src='/images/maze-generalization.png' alt='Models trained on easy mazes are tested on much harder ones'>"
collection: portfolio
date: 2026-01-22
---

**Published at AAAI 2026** &middot; [Paper](https://arxiv.org/abs/2410.03020) &middot; [Code](https://github.com/mines-opt-ml/maze-extrapolation) &middot; [Talk video](https://underline.io/lecture/139985-on-logical-extrapolation-for-mazes-with-recurrent-and-implicit-networks) &middot; [Slides](/files/aaai26-slides.pdf) &middot; [Poster](/files/aaai26-poster.pdf) &middot; [maze-dataset package](https://github.com/understanding-search/maze-dataset)

_Joint work with Amandin Chyba Rabeendran, Michael Ivanitskiy, Jordan Pettyjohn, Cecilia Diniz-Behn, Samy Wu Fung, and Daniel McKenzie._

**TL;DR:** Some neural networks can "think longer" at test time by iterating more. I stress-tested whether that extra compute lets them solve problems _harder_ than anything they saw in training. It does — in some ways but not others: the models quietly learned a shortcut heuristic instead of a general algorithm, and diversifying the training data fixed some failures while creating others.

## The problem

Production models constantly face inputs harder than their training data. Recurrent networks (RNNs) and implicit networks (INNs) offer a tempting fix: their depth is adjustable at test time, so they can spend more compute on harder inputs — like a person thinking longer about a harder puzzle. I investigated when this actually works and how it fails.

<img class="fig" src="/images/maze-generalization.png" alt="Train on small, easy mazes; test on large mazes and mazes with loops">

Maze-solving is the ideal testbed: classical algorithms provide ground-truth solutions at any difficulty, so I trained on easy mazes and generated _harder_ test mazes along three controlled dimensions. **Maze size** (e.g., 9×9) makes problems bigger. **Percolation** (_p_ = 0 to 1) removes walls to create loops, producing multiple solutions where previously there was one. **Deadend-start** (True or False) controls whether the solution path begins at a dead end. The models were trained only on the easiest corner of this space — 9×9 mazes with _p_ = 0 and deadend-start True — then tested far beyond it. All mazes come from [maze-dataset](/portfolio/maze-dataset/), the open-source package we built for exactly this purpose.

<img class="fig" src="/images/maze-ood-shifts.png" alt="Three axes of increasing test difficulty: maze size, percolation, and start position">

## What I did

- Evaluated a pre-trained RNN from [Bansal et al.](https://arxiv.org/abs/2202.05826) and a custom-trained INN across thousands of out-of-distribution mazes, varying maze size, percolation, and deadend-start.
- Investigated _what the models actually learned_ by studying their failures: correct answers all look alike, but failures have signatures. This is how we found that the RNN had approximately learned _dead-end filling_ (a classic maze-solving algorithm). The INN matched no algorithm we tested.
- Used topological data analysis to rigorously measure whether the networks' internal computations converge to a fixed point, or something else.
- Retrained both architectures across a range of training-data diversity levels, against a standard network 10× the size, to assess how diversification affects generalization.

## What I found

**1. Test-time compute works — dramatically.** With enough iterations, the RNN solves mazes 10× larger than anything in training at near-perfect accuracy, and both iterative models beat a standard network 10× their size.

<img class="fig fig--wide" src="/images/maze-extrapolation.png" alt="Accuracy vs. maze size: more test-time iterations extend near-perfect accuracy to much larger mazes">

**2. But the model learned a heuristic, not the goal.** The RNN's predictions match _dead-end filling_, a classic algorithm that provably fails on mazes with loops — and indeed the RNN fails on them, incorrectly retaining loops from the input maze in its prediction. This is goal misgeneralization: perfect training accuracy concealed that the model never learned a general maze-solving method, only a shortcut that fails on mazes with loops.

<img class="fig" src="/images/maze-rnn-loop-failure.png" alt="On a maze with loops, the RNN's prediction retains loops instead of a valid path">

**3. Data diversity is a trade-off, not a free lunch.** I diversified the training data by raising its percolation above _p_ = 0, letting the models see mazes with loops during training. Even _p_ = 0.003 unlocked loop-solving — but as training percolation grew, accuracy concentrated around the training distribution and size extrapolation _degraded_.

<img class="fig fig--wide" src="/images/maze-combined-heatmaps.png" alt="Test accuracy heatmaps: diversifying training data helps loops but hurts size extrapolation">

**4. A standing theoretical assumption was contradicted.** Prior work assumed iterates must converge to a fixed point for extrapolation. Topological analysis revealed the RNN often converges to cycles instead while still achieving perfect accuracy — behavior that standard residual plots hide, but PCA projections expose.

<img class="fig" src="/images/maze-residual-pca.png" alt="Residual plots look flat or noisy, but PCA projections reveal the iterates cycling between two points (top) or around two loops (bottom)">

## Why it matters beyond mazes

Every finding is a production-ML lesson in miniature: in-distribution accuracy can hide the fact that a model learned the wrong thing; evaluation must probe _multiple_ axes of distribution shift; and "add more diverse data" reallocates capability rather than uniformly adding it. The evaluation framework — controlled difficulty dimensions with algorithmic ground truth — is the same discipline behind any serious model-validation pipeline.

_Skills: Python/PyTorch, large-scale experiment design, out-of-distribution evaluation & stress-testing, model debugging & failure analysis, topological data analysis, scientific communication (peer-reviewed AAAI paper + conference talk)._
