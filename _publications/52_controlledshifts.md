---
layout: default
title: "ControlledShifts: Towards Standardizing Robustness Evaluation in Trajectory Prediction Under Distribution Shifts"
blogpost_link: /controlledshifts/
paper_url: https://arxiv.org/abs/2608.17882
poster: null
code: https://github.com/navarrs/ControlledShifts
video: null
thumbnail: assets/img/publications/controlledshifts.png
authors: <b class="text-primary">Ingrid Navarro†</b>, Pablo Ortega-Kral†, Yutong Duan, Jonathan Francis‡ and Jean Oh‡
note: "† Work done as part of an internship at <b>Lavoro AI</b>; ‡ Equal advising"
where: Preprint in ArXiv, 2026
id: paper_controlledshifts
abstract: "
<p>Trajectory prediction is central to safety in autonomous driving, yet learning-based predictors tend to degrade
sharply when encountering scenarios poorly represented by their training data. Many methods attempt to mitigate
distribution shift degradation through data-centric or test-time adaptation approaches; however, they are typically
validated along fragmented axes of generalization, leaving the field without a standardized way to compare robustness
across shifts a model may encounter. </p>
<p>To address this, we introduce <b>ControlledShifts</b>, a framework and benchmark suite that systematically
re-splits existing trajectory datasets into in-distribution (SEEN) and out-of-distribution (UNSEEN) partitions, via a
shared <i>characterization-and-splitting</i> formulation, in which a characterization function fixes the axis of
variation a benchmark probes and a splitting function fixes how the tail of that axis is withheld. The suite comprises
three benchmarks targeting key topological and behavioral distribution shifts. Furthermore, to aggregate
multi-dimensional performance metrics across these benchmarks, we propose a unified robustness score that evaluates
models along two complementary dimensions: prediction <i>quality</i> (relative performance gain) and prediction
<i>stability</i> (performance preservation under shift). We showcase ControlledShifts by benchmarking prominent
transformer-based architectures, exposing critical differences in how models of varying capacities handle latent
relevance and environmental structure. </p>
"
---
