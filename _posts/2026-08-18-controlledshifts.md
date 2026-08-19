---
layout: post
author: Ingrid Navarro
permalink: /controlledshifts/
title: ControlledShifts
subtitle: "Towards Standardizing Robustness Evaluation in Trajectory Prediction Under Distribution Shifts"
image: /assets/posts/2026-08-18-controlledshifts/teaser.png
link-new-tab: true
categories: publication
hero_image: /assets/img/background.gif
hero_height: is-small
---

<h1>
ControlledShifts: Towards Standardizing Robustness Evaluation in Trajectory Prediction Under Distribution Shifts
</h1>

[Ingrid Navarro](https://navars.xyz)†, Pablo Ortega-Kral†, Yutong Duan,
[Jonathan Francis](https://scholar.google.com/citations?user=7CLS0LwAAAAJ&hl=en)‡ and
[Jean Oh](https://www.cs.cmu.edu/~jeanoh/)‡

†Work done as part of an internship at **Lavoro AI**

‡Equal advising

<a class="button" itemprop="github" href="https://github.com/navarrs/ControlledShifts" target="_blank">
  <i class="fab fa-github fa-lg"></i>
</a>
<a class="button" itemprop="paper" href="https://arxiv.org/abs/2608.17882" target="_blank">
  <i class="fas fa-file fa-lg"></i>
</a>

# Abstract

Trajectory prediction is central to safety in autonomous driving, yet learning-based predictors tend to degrade
sharply when encountering scenarios poorly represented by their training data. Many methods attempt to mitigate
distribution shift degradation through data-centric or test-time adaptation approaches; however, they are typically
validated along fragmented axes of generalization, leaving the field without a standardized way to compare
robustness across shifts a model may encounter.

To address this, we introduce **ControlledShifts**, a framework and benchmark suite that systematically re-splits
existing trajectory datasets into in-distribution (SEEN) and out-of-distribution (UNSEEN) partitions, via a shared
**characterization-and-splitting** formulation, in which a characterization function fixes the axis of variation a
benchmark probes and a splitting function fixes how the tail of that axis is withheld. The suite comprises three
benchmarks targeting key topological and behavioral distribution shifts. Furthermore, to aggregate multi-dimensional
performance metrics across these benchmarks, we propose a **unified robustness score** that evaluates models along
two complementary dimensions: prediction **quality** (relative performance gain) and prediction **stability**
(performance preservation under shift). We showcase ControlledShifts by benchmarking prominent transformer-based
architectures, exposing critical differences in how models of varying capacities handle latent relevance and
environmental structure.

<p align="center">
  <img width="1280" src="/assets/posts/2026-08-18-controlledshifts/teaser.png" alt="Overview">
</p>
<p align="center">
<caption><i>Overview of ControlledShifts. <b>Controlled Shift Induction</b> (top): a characterization function c maps
each scenario to a desired representation, and a splitting function f withholds the tail of the resulting
distribution, giving SEEN (train/val) and UNSEEN (test) partitions instead of a uniform random split. The toy cut-in
example shown here is characterized three ways, yielding the same corpus under three different distributions and
held-out tails. <b>Unified Robustness Evaluation</b> (bottom): predictors are trained and evaluated on this benchmark
pool, and their results are unified into quality and stability scores.</i></caption>
</p>

<hr>

# Motivation

At deployment time, autonomous systems inevitably face novel situations their training data covers poorly: unseen
road topologies, unrelated background actor behavior, unexpected maneuvers, or erratic driving. Safety demands that a
model respond sensibly to these cases, yet learning-based predictors generally degrade under such distribution shifts.

Efforts to improve robustness fall into two categories. The first targets the data, through scenario mining, as in our
own [SafeShift](https://navars.xyz/safeshift/), or through coverage widening via scenario generation, as in
[SEAL](https://navars.xyz/seal/). The second acts at test time, through uncertainty estimation, model ensembles or
gradient-based shift detection.

Despite this activity, measuring how robust a model actually is remains an *ad hoc* exercise. Each method commits to
its own axis of generalization, so robustness cannot be contextualized across methods and shift conditions. Fields
such as robotics have benchmarks that aggregate distinct sources of variation; trajectory prediction in autonomous
driving lacks definitive efforts towards such standardization.

### Our Idea

We propose **ControlledShifts**, a framework and benchmark suite that, rather than collecting new data, re-splits an
existing dataset into SEEN and UNSEEN partitions through a shared formulation instantiated across distinct axes of
variation. Our contributions are threefold.

1. We **formalize and unify distribution-shift creation** by combining a characterization function and a splitting
   function into one standardized formulation for inducing controlled shifts across heterogeneous axes.
2. We propose a **unified robustness score** condensing multi-dimensional measurements into two values per model,
   along the **quality** and **stability** dimensions.
3. We instantiate the formulation as **three complementary benchmarks** targeting behavioral and topological shifts
   relevant to the ego-agent, and showcase our robustness scheme on key trajectory prediction models.

<hr>

# Inducing Controlled Shifts

To create a benchmark, we partition the scenario set into two disjoint subsets. A **characterization function** `c`
maps each scenario to a representation, and a **splitting function** `f` assigns each scenario to SEEN or UNSEEN
based on that representation, relative to the full corpus. Models are trained and validated exclusively on SEEN,
while UNSEEN is held out for testing.

Here `c` determines the axis of variation a benchmark probes and `f` how its tail is withheld, so either can be
replaced without disturbing the other. Because a benchmark is a re-splitting of existing data, scenario format and
quality are held fixed, making the induced shift the only variation; benchmarks built from different characterization
functions remain comparable, being re-splits of the same corpus scored the same way; and adding an axis requires only
a new `(c, f)` pair.

<hr>

# The Benchmark Suite

We instantiate our formulation as three benchmarks, each targeting a different aspect in which a scenario can be
*interesting* to the ego-agent, plus a no-shift control.

| Benchmark | Characterization `c` | Splitting `f` | Axis probed |
|---|---|---|---|
| BackgroundAgents | Background-agent count | Top 15% held out | Distractor density |
| EgoSafeShift | Ego-centric safety score | 85th percentile | Latent criticality |
| Environments | NetLSD map-graph embedding | Highest-silhouette clusters | Map topology |
| Uniform | — | Uniform at random | None (control) |

### BackgroundAgents

This benchmark builds on **CausalAgents**, in which human annotators labeled the agents causal to the ego-agent's
behavior, leaving the remaining ones as *background* agents. Masking those out should ideally leave predictions for
the ego-agent unchanged. Our characterization counts them, a proxy for perturbation magnitude, and the splitting
function holds out the top 15% highest-count scenarios, whose perturbed counterparts form UNSEEN, while the remainder
constitutes SEEN in its original form.

### EgoSafeShift

This benchmark adapts the characterization scheme of [SafeShift](https://navars.xyz/safeshift/) and
[ScenarioCharacterization](https://navars.xyz/scenario-characterization/), assigning each scenario a scalar
safety-relevance score and thresholding at the 85th percentile. Where the original formulation captures global
safety-related properties, ours targets finer-grained characteristics that increase decision-making difficulty, so we
re-score existing scenarios **from the ego-agent's perspective**.

### Environments

This benchmark induces a topological shift by holding out scenarios with structurally distinct maps. We convert each
scenario map into a **NetworkX** graph and embed it with **NetLSD**, a heat-trace graph descriptor that is permutation
invariant, scale adaptive, size invariant and scalable. Agglomerative clustering over the embeddings then assigns the
highest-silhouette clusters to UNSEEN: a cleanly separated cluster is internally homogeneous and structurally distinct
from the rest of the corpus, and is therefore precisely its low-diversity, topologically atypical region.

<p align="center">
  <img width="800" src="/assets/posts/2026-08-18-controlledshifts/benchmarks.png" alt="Benchmarks">
</p>
<p align="center">
<caption><i>Representative SEEN (left, green panes) and UNSEEN (right, red panes) scenarios for each benchmark:
BackgroundAgents (top), EgoSafeShift (center), and Environments (bottom). SEEN scenarios are used for training and
validation; UNSEEN scenarios are held out for testing.</i></caption>
</p>

<hr>

# Measuring Robustness

Given an induced shift, the goal is to minimize performance degradation from SEEN to UNSEEN. Degradation on its own,
however, is insufficient: a weak predictor can degrade little simply because it had little to lose. We therefore
measure robustness along two dimensions, each fixed by the **reference** a predictor is compared against.

* **Quality** captures downstream performance, measuring a model against a **Naive** baseline given neither map nor
  social context.
* **Stability** captures how well a model maintains its performance under shift, measuring it against its own
  counterpart trained on the **Uniform** setting, where no controlled shift is applied.

The score is built in three steps. For a given metric, the **split ratio** divides the reference's score by the
predictor's on that split; every metric is an error measure, so a ratio above 1 means the predictor beats the
reference. The **per-benchmark score** combines the SEEN and UNSEEN ratios via a geometric mean, which penalizes
imbalance between the two, and averages across metrics. The **unified robustness score** then averages those over the
three shifted benchmarks, giving one value per predictor and dimension.

<hr>

# Results

We validate on the Waymo Open Motion Dataset (WOMD), using a subset of ~45K scenarios re-split 70% / 15% / 15% under
each benchmark. Every model is trained once per benchmark, so all differences are attributable to the induced shift.
We evaluate four transformer-based baselines spanning diverse capacities, **AutoBot** (1.5M parameters),
**SceneTransformer** (7.6M), **Wayformer** (15.1M) and **MTR** (27.2M), against the **Naive** reference (624k), on the
standard MinADE<sub>6</sub>, MinFDE<sub>6</sub>, BrierFDE, MissRate and CollisionRate metrics.

Under the Uniform condition, models lose less than 1% on average, while the proposed shifts degrade them
substantially, and each in a distinct way.

* **BackgroundAgents** produces substantial behavioral degradation, with MinADE<sub>6</sub> rising 23.8% on average,
  yet a marked *drop* in collision rate (−21.9%), since predictions rarely collide with non-background agents.
* **Environments** shows the same signature, +12.7% MinADE<sub>6</sub> against −5.4% collision rate. The held-out
  topologies demand less interaction understanding and maneuvering, but still require sound longitudinal operation.
* **EgoSafeShift** shows the smallest gap in the FDE-based metrics (+10.9%) but the largest increase in collision rate
  (+25.6%). The UNSEEN set surfaces latent criticality, demanding a model's grasp of safety relevance.

<p align="center">
  <img width="1280" src="/assets/posts/2026-08-18-controlledshifts/robustness_rankings.png" alt="Rankings">
</p>
<p align="center">
<caption><i>Robustness scores, showing quality (left) and stability (right) rankings across models. Higher is
better.</i></caption>
</p>

The two rankings turn out to be close to inverses of one another. This is not by construction, as a predictor could
lead on both dimensions, so it validates that changing the reference captures complementary aspects of robustness
rather than a single underlying ordering.

On **quality**, Wayformer ranks highest, outperforming on displacement error metrics across benchmarks. However, like
SceneTransformer and MTR, it falls below the Naive reference on collision rate, suggesting that the accuracy gains of
high-capacity models are largely confined to the geometry of the modal trajectory and do not translate into
proportionate improvements in safety-aligned metrics. On **stability**, every learned model degrades more than Naive,
and the pattern inverts: the higher-capacity models lose displacement error stability while improving on collision
rate, whereas AutoBot degrades on every metric and scores lowest, despite being the strongest model on collision rate
throughout. **Relative degradation alone is therefore a misleading proxy for downstream reliability.**

<hr>

<h4 align="center">
<span style="color:blue">Check out our paper and the benchmark suite for more details!</span>
</h4>

# BibTeX

```
@article{navarro2026controlledshifts,
  title={ControlledShifts: Towards Standardizing Robustness Evaluation in Trajectory Prediction Under Distribution Shifts},
  author={Navarro, Ingrid and Ortega-Kral, Pablo and Duan, Yutong and Francis, Jonathan and Oh, Jean},
  journal={arXiv preprint arXiv:2608.17882},
  year={2026}
}
```

<hr>
