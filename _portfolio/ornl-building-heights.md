---
title: "Predicting Building Heights from Satellite-Derived Footprints"
excerpt: "At Oak Ridge National Laboratory, I trained an XGBoost model on 898,000 buildings to predict building heights from footprint features (0.70 m MAE), and showed that adding coarse global height data doesn't help.<br/><img src='/images/ornl-footprints.png' alt='Satellite imagery of Denver converted to building footprints'>"
collection: portfolio
date: 2023-07-01
---

**Machine learning internship, Oak Ridge National Laboratory (GeoAI group), Summer 2023** &middot; [Poster](/files/ornl_poster.pdf)

_Joint work with Clinton Stipek, Taylor Hauser, and Robert Stewart (mentor)._

**TL;DR:** Accurate building heights feed important models of population, energy efficiency, and climate risk, and inform urban planning — but the LIDAR that measures them directly is too expensive for most of the world. I helped build a machine learning pipeline at Oak Ridge National Laboratory (ORNL) that predicts individual building heights from satellite-visible footprint features, reaching **0.70 m mean absolute error** across 898,000 Colorado buildings, and ran a controlled experiment showing that a widely-used global height dataset adds essentially nothing to prediction accuracy.

## The problem

How many people live on each block of a city? How exposed is a neighborhood to flooding? Where should a growing city build next? Answering questions like these requires an accurate inventory of the buildings that already exist, including how tall they are ([Biljecki et al., 2015](https://doi.org/10.3390/ijgi4042842)).

LIDAR height measurements are the gold standard, but only wealthy cities can afford the fly-over scans. By comparison, satellite imagery is cheap and global, but it doesn't measure building heights directly. However, satellite imagery does allow for computation of building _footprints_, traces of the perimeters of individual buildings. There are a variety of secondary quantities that can be extracted from building footprints, like area, perimeter, number of nearby buildings, and so on, that are highly predictive of building height. ORNL has compiled a comprehensive list of footprint features using a tool called [Gauntlet](https://github.com/ORNL/gauntlet).

During this internship, I helped build a machine learning pipeline that used these footprint features to predict building heights remarkably well. However, this model had a flaw shared by many satellite-derived approaches: inaccurate estimation of tall buildings. This is especially problematic because urban areas cover a tiny fraction of the world's surface but contain most of its people.

The open question: does adding the best available coarse-scale global building height estimate, the European Commission's 100 m Average Net Building Height (ANBH), improve predictions beyond what footprints alone provide, especially for tall buildings?

<img class="fig fig--wide" src="/images/building-height-prediction.jpg" alt="Pipeline: satellite imagery of Denver becomes building footprints and footprint features, which combine with coarse ANBH height data to produce building height predictions">

## What I did

- Helped build a data-processing pipeline joining ORNL's vector footprint features (65 per building) with the raster ANBH data, using **PostGIS** within **QGIS**, containerized with **Docker**.
- Trained **XGBoost** models on 898,000 LIDAR-measured Colorado buildings (80/20 train/test): some on footprint features alone, and others with footprint features + ANBH.
- Benchmarked against median and linear-regression baselines.

## What I found

**Footprint features carry nearly all the signal.** The footprint-only model achieved 0.698 m MAE, versus 1.15 m for the median baseline. Adding ANBH improved the error only marginally (0.698 → 0.695 m MAE), with no improvement on tall buildings.

**ANBH alone is inaccurate.** Audited directly against LIDAR ground truth, ANBH showed 3.09 m MAE, nearly 3× worse than simply predicting the median. Its distribution barely resembles the truth (and reveals truncation below 2.5 m).

<img class="fig fig--wide" src="/images/true-vs-anbh-heights.png" alt="Histograms of true LIDAR building heights versus ANBH values: the true heights peak near 4 m while ANBH mass sits near 8 m with a large artifact spike at 2.5 m">

**ANBH is redundant.** Despite adding no accuracy, XGBoost ranked ANBH its 4th most important feature. So ANBH does carry genuine height information, just nothing _beyond_ what the strongest footprint features already encode. Inaccurate at the building scale, ANBH offers no marginal value over footprint features.

## Why it matters beyond buildings

This internship was my first taste of data science as a daily job. The data was messy and real, and the deliverable was a recommendation rather than a paper. I arrived new to geospatial science and picked up its tools on the job, from the domain-specific (PostGIS, a geospatial flavor of SQL) to the general-purpose (Docker). Three lessons stuck with me: always start with a baseline, check the inputs before trusting the outputs, and remember that an honest, well-supported "no" is a useful deliverable.

## A Summer in Tennessee

<div class="photo-row">
  <img src="/images/ornl.jpeg" alt="With my mentor Robert Stewart beside our poster at the ORNL intern symposium">
  <img src="/images/tennessee.jpeg" alt="The Smoky Mountains from a hiking trail in East Tennessee">
</div>

This project was completed during my summer 2023 internship at Oak Ridge National Laboratory in Tennessee, where I had the good fortune of being mentored by [Robert Stewart](https://www.ornl.gov/staff-profile/robert-n-stewart). Oak Ridge was a fantastic place to spend the summer, filled with great people and surrounded by a beautiful tapestry of meandering rivers and rolling green hills.

_Skills: XGBoost, geospatial data engineering (PostGIS, QGIS, vector/raster), Docker, GitLab, experiment design, baseline benchmarking, communicating negative results._
