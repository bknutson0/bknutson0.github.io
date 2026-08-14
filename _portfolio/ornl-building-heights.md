---
title: "Predicting Building Heights from Satellite-Derived Footprints"
excerpt: "At Oak Ridge National Laboratory, I trained an XGBoost model on 898,000 buildings to predict building heights from footprint features (0.70 m MAE), and showed that adding coarse global height data doesn't help.<br/><img src='/images/ornl-footprints.png' alt='Satellite imagery of Denver converted to building footprints'>"
collection: portfolio
date: 2023-07-01
---

**Machine learning internship, Oak Ridge National Laboratory (GeoAI group), Summer 2023** &middot; [Poster](/files/ornl_poster.pdf)

_Joint work with Clinton Stipek, Taylor Hauser, and Robert Stewart (mentor)._

**TL;DR:** Accurate building heights feed models of population, energy efficiency, and climate risk — but the LIDAR that measures them directly is too expensive for most of the world. I built a machine learning pipeline at ORNL that predicts individual building heights from satellite-visible footprint features, reaching **0.70 m mean absolute error** across 898,000 Colorado buildings, and ran a controlled experiment showing that a widely-used global height dataset adds essentially nothing to prediction accuracy.

<img class="fig" src="/images/ornl-footprints.png" alt="Satellite imagery of Denver, CO converted to building footprints">

## The problem

LIDAR height measurements are the gold standard but only wealthy cities can afford the scans. Satellite imagery is cheap and global — and while it doesn't measure height, the building *footprints* visible in it (area, perimeter, neighborhood density) are highly predictive of height. The open question: does adding the best available coarse-scale global height product — the European Commission's 100 m Average Net Building Height (ANBH) — improve predictions beyond what footprints alone provide, especially for tall buildings?

## What I did

- Built the data-processing pipeline for vector and raster geospatial data using **PostGIS**, **QGIS**, and **Docker**, on ORNL's GAUNTLET footprint-feature dataset (65 features per building).
- Trained **XGBoost** models on 898,000 LIDAR-verified Colorado buildings (80/20 train/test) — Colorado chosen for its rural-to-urban diversity — in two controlled configurations: footprint features alone, and footprint features + ANBH.
- Benchmarked against median and linear-regression baselines, evaluating with MAE, RMSE, and R².

## What I found

**Footprint features carry nearly all the signal.** The footprint-only model achieved 0.698 m MAE (vs. 1.15 m for the median baseline). Adding ANBH moved the metrics by rounding error: MAE 0.698 → 0.695 m, RMSE 1.134 → 1.128 m, R² 0.528 → 0.533.

**And I could explain why.** Auditing ANBH directly against LIDAR ground truth showed the coarse data itself was poor: 3.09 m MAE — nearly 3× worse than simply predicting the median. A negative result, but a decision-ready one: teams building global height models shouldn't spend integration effort on ANBH.

## Why it matters beyond buildings

This is the everyday shape of industry data science: a plausible-sounding data source, a controlled experiment to price its value, and a clear recommendation backed by an audit of *why* the result came out the way it did. The same discipline — baseline first, ablate one variable, diagnose the mechanism — is what separates "we added the data" from "we know what the data is worth."

*Skills: XGBoost, geospatial data engineering (PostGIS, QGIS, vector/raster), Docker, GitLab, experiment design, baseline benchmarking, communicating negative results.*
