---
layout: post
author: Ingrid Navarro
permalink: /scenario-characterization/
title: ScenarioCharacterization
subtitle: "A Modular Toolkit for Characterizing Safety across Trajectory Datasets"
image: /assets/posts/2026-08-17-scenario-characterization/teaser.png
link-new-tab: true
categories: publication
hero_image: /assets/img/background.gif
hero_height: is-small
---

<h1>
ScenarioCharacterization: A Modular Toolkit for Characterizing Safety across Trajectory Datasets
</h1>

[Ingrid Navarro](https://navars.xyz)†, Yutong Duan,
[Jonathan Francis](https://scholar.google.com/citations?user=7CLS0LwAAAAJ&hl=en)‡ and
[Jean Oh](https://www.cs.cmu.edu/~jeanoh/)‡

†Developed in part during an internship at **[Stack AV](https://stackav.com)**

‡Equal advising

<a class="button" itemprop="github" href="https://github.com/navarrs/ScenarioCharacterization" target="_blank">
  <i class="fab fa-github fa-lg"></i>
</a>
<a class="button" itemprop="paper" href="https://arxiv.org/abs/2608.16041" target="_blank">
  <i class="fas fa-file fa-lg"></i>
</a>

# Abstract

We introduce **ScenarioCharacterization**, an open-source framework for automated, dataset-agnostic profiling of
driving scenarios in trajectory datasets. Our framework is packaged as a modular, configuration-driven pipeline
of three layers: a **dataset adapter** that maps custom datasets onto an open Scenario representation, a
**characterizer** that performs feature extraction, behavior probing, and criticality scoring at scenario and
agent levels, and an **analysis** layer for scenario visualization and feature, score, and probe analyses.
Because the layers communicate only through Pydantic-validated schemas composed via configurations, a new
dataset can easily plug in without rewriting the characterization and analysis stack.

<p align="center">
  <img width="1280" src="/assets/posts/2026-08-17-scenario-characterization/teaser.png" alt="Overview">
</p>
<p align="center">
<caption><i>Overview of ScenarioCharacterization. Colored blocks denote the schemas that carry data between
layers. Solid arrows mark required paths, dashed arrows optional ones.</i></caption>
</p>

<hr>

# Motivation

Finding which scenarios in a driving dataset are interesting or safety-relevant is an important problem in
autonomous driving research. It underpins efforts in robustness evaluation, dataset splitting, curriculum design
and agent relevance labeling.

Much of the machinery used for these tasks, however, is tailored to a specific setting and dataset. Prior work,
including our own [SafeShift](https://navars.xyz/safeshift/), proposes characterization logic that is tightly
coupled to a single dataset's file format, agent taxonomy, sampling rate and map schema. Moving to a new dataset
means rewriting the feature extractors, the scorers and the analysis code alongside the loader. It also makes
cross-dataset comparison difficult.

### Our Idea

We propose **ScenarioCharacterization**, a configuration-driven library that separates *what* is characterized
from *where the data came from*. The library makes three contributions.

1. A **scenario profiling pipeline** that consolidates characterization components from prior work into a single
   reusable library.
2. A **schema-based model** that maps heterogeneous datasets onto a unified representation, so downstream stages
   operate independently of the source benchmark.
3. **Worked examples** on WOMD, Argoverse2 and nuPlan, plus two downstream uses, scenario mining and agent
   labeling.

<hr>

# Framework

The pipeline is split into three layers. They communicate only through Pydantic-validated schemas, composed via
Hydra configurations. Each component is extended by subclassing its base class and implementing one entry point.

| Component | Base Class | Entry point | Output |
|---|---|---|---|
| Dataset Adapter | `BaseDataset` | `transform(Raw Scenario)` | `Scenario` |
| Feature Extractor | `BaseFeature` | `compute(Scenario)` | `ScenarioFeatures` |
| Scorer | `BaseScorer` | `compute(Scenario, ScenarioFeatures)` | `ScenarioScores` |
| Behavior Prober | `BehaviorProber` | `probe(Scenario)` | `ScenarioProbe` |
| Visualizer | `BaseVisualizer` | `visualize(Scenario)` | Rendered Scenario |

### Dataset Adapter → `Scenario`

The adapter is the only dataset-specific code. It maps a raw sample onto a common `Scenario` object. That object
is built from five parts.

* `ScenarioMetadata` holds identifiers, timing and thresholds.
* `AgentData` holds the trajectories, agent types and relevance flags.
* `TracksToPredict` marks the subset of agents targeted for prediction or navigation.
* `StaticMapData` describes lanes, geometry, regulatory elements and conflict points.
* `DynamicMapData` covers traffic lights and other time-varying map features.

We provide adapters for WOMD, Argoverse2, nuPlan and nuScenes. A new dataset only requires implementing
`transform`.

### Feature Extractor → `ScenarioFeatures`

The extractor computes **low-level features**. They are grouped into one container per axis.

* `individual_features` covers single-agent state and behavior. It holds speed, acceleration and jerk,
  speed-limit difference, waiting period at a conflict point, trajectory type and Kalman difficulty.
* `interaction_features` covers agent-pair conflicts. It holds time-to-collision (TTC), time headway (THW),
  deceleration rate to avoid a crash (DRAC), minimum time to conflict point (mTTCP) and collisions.

Each axis also carries validity masks and the agent types the features were computed over. Downstream stages can
then filter by agent class without going back to the `Scenario`. New axes slot in alongside these two.

### Scorer → `ScenarioScores`

The scorer collapses features into scores. `ScenarioScores` holds one `Score` per aggregator, namely
`individual_scores`, `interaction_scores` and `safeshift_scores`. Each `Score` carries per-agent scores, their
validity mask and a single aggregated `scene_score`.

The three aggregators are interchangeable. The **individual** one importance-weights clipped individual features
per agent. The **interaction** one does the same per agent pair. The **SafeShift** one combines the two
hierarchically, weighted by proximity. It also extends the original formulation with a vulnerable-road-user
multiplier.

### Behavior Prober → `ScenarioProbe`

The prober searches for latent criticality that the recorded behavior hides. A constant-velocity probe replaces
an agent's future with a constant-velocity extrapolation, emulating a non-reactive, distracted driver. A
score-based validator then re-scores the perturbed scenario on the interaction axis. It retains the probe only
when the per-pair delta exceeds a threshold. The sweep covers the ego against all others and each non-ego agent
against the ego, keeping the single most impactful probe.

The resulting schema records `probed_agent_id` and `probed_agent_trajectory`, whether the probed agent
`is_ego_agent`, the `probe_type`, and the `criticality_time_idx` at which the perturbed scenario is most
critical. It also stores the scene score before and after probing, along with the affected agent ids and their
pairwise scores on both sides of the perturbation. If no candidate is valid, the prober returns an empty probe.

<hr>

# Analysis

The analysis layer consumes the schemas above. It renders scenarios either as static images or as animations, with panes
for all agents, relevant agents, agents shaded by criticality and counterfactual probes. It also turns the
per-scenario outputs into distributional views, both within and across datasets.

<p align="center">
  <img width="1280" src="/assets/posts/2026-08-17-scenario-characterization/scenario_viz.png" alt="Visualization">
</p>
<p align="center">
<caption><i>Supported visualizations, shown on two WOMD scenarios. On the top, the ego waits at a traffic light
while relevant agents approach and stop behind it. On the bottom, the ego approaches an intersection as a
cyclist initiates a left turn across its path.</i></caption>
</p>

<p align="center">
  <img width="1280" src="/assets/posts/2026-08-17-scenario-characterization/scenario_animation.gif" alt="Animated scenario">
</p>
<p align="center">
<caption><i>The same four panes, animated on a single WOMD scenario. From left to right: all agents, relevant
agents highlighted, agents shaded by criticality, and the counterfactual probe, which perturbs agent 247 and
raises the pair score against the affected agent 347.</i></caption>
</p>

For the distributional views below we ran the pipeline over 5,000 scenarios from each of WOMD, Argoverse2 and
nuPlan. All were standardized to 10 Hz, with agent types mapped onto a common taxonomy.

<p align="center">
  <img width="1280" src="/assets/posts/2026-08-17-scenario-characterization/feature_analysis.png" alt="Features">
</p>
<p align="center">
<caption><i>Feature analysis. On the top, speed-limit difference distributions per agent type on WOMD, with the
25th, 75th and 90th percentiles marked. On the bottom, DRAC distributions across WOMD, nuPlan and
Argoverse2.</i></caption>
</p>

Feature-level views are what percentile-based binning is built on. Cross-dataset views expose how comparable two
benchmarks really are. DRAC, for instance, behaves consistently everywhere. Between 60 and 80% of interactions
sit at zero, with a decaying tail beyond.

<p align="center">
  <img width="1280" src="/assets/posts/2026-08-17-scenario-characterization/score_analysis.png" alt="Scores">
</p>
<p align="center">
<caption><i>Score analysis. On the top, score distributions across the supported aggregators on WOMD. On the
bottom, the SafeShift score across WOMD, nuPlan and Argoverse2.</i></caption>
</p>

Score-level views separate the datasets more sharply. WOMD carries the densest vehicle traffic and the heaviest
SafeShift tail, which extends past 10. Argoverse2 concentrates at the low end. nuPlan sits in between, with
fewer vehicles but far more pedestrians.

<p align="center">
  <img width="600" src="/assets/posts/2026-08-17-scenario-characterization/probe_outcomes_combined.png" alt="Probes">
  <img width="600" src="/assets/posts/2026-08-17-scenario-characterization/score_scatter_combined.png" alt="Deltas">
</p>
<p align="center">
<caption><i>On the left, probe outcomes per dataset, either no critical probe or one perturbing the ego or a
non-ego agent. On the right, the per-scenario interaction score before and after probing. Vertical displacement
from the dashed identity line is the probe-induced delta.</i></caption>
</p>

Probing reshapes these distributions rather than shifting them uniformly. The share of scenarios yielding an
ego-based critical probe varies substantially with dataset density. WOMD reaches 34.4% against 19.7% on nuPlan.
That gap is a useful signal in itself when deciding which benchmark to mine.

<hr>

# Downstream Uses

### Agent Criticality Categorization

Because feature and score distributions are available across the whole dataset, percentile-based binning
partitions them into low-to-high criticality bands. The resulting categorical labels can serve as auxiliary
supervision, drive curriculum learning or flag agent relevance. They also improve model interpretability.

<p align="center">
  <img width="1280" src="/assets/posts/2026-08-17-scenario-characterization/categorized_agents.png" alt="Categories">
</p>
<p align="center">
<caption><i>Agents colored from low (green) to high (red) criticality, with the ego-agent in blue.</i></caption>
</p>

### Scenario Mining

The same distributions support mining at any point along the criticality spectrum. Mining the tail surfaces
recorded scenarios worth stress-testing against. Mining by **probe-induced delta** surfaces scenarios that look
benign as recorded but become critical under a plausible behavioral deviation.

<p align="center">
  <img width="1280" src="/assets/posts/2026-08-17-scenario-characterization/scenario_mining.png" alt="Mining">
</p>
<p align="center">
<caption><i>Mining by probe-induced criticality delta. On the left, scores before and after probing, color-coded
by whether the probed agent is the ego. On the right, the interaction marked with a red ×, shown before and
after probing. There the constant-velocity probe raises the pair score by 10.5.</i></caption>
</p>

<hr>

<h4 align="center">
<span style="color:blue">Check out our paper and the library for more details!</span>
</h4>

# BibTeX

```
@article{navarro2026scenariocharacterization,
  title={ScenarioCharacterization: A Modular Toolkit for Characterizing Safety across Trajectory Datasets},
  author={Navarro, Ingrid and Duan, Yutong and Francis, Jonathan and Oh, Jean},
  journal={arXiv preprint arXiv:2608.16041},
  year={2026}
}
```

<hr>
