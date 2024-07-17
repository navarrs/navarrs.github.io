---
layout: post
author: Ingrid Navarro
permalink: /safeshift/
title: SafeShift
# subtitle: "SafeShift: Safety-informed Distribution Shift for Robust Trajectory Prediction in 
# Autonomous Driving"
image: /assets/posts/2024-05-13-safeshift/model.png
link-new-tab: true
categories: publication
hero_image: /assets/img/background.gif
hero_height: is-small
---

<h1> 
SafeShift: Safety-informed Distribution Shift for Robust Trajectory Prediction in Autonomous Driving 
</h1>

[Benjamin Stoler](https://benstoler.com) *, [Ingrid Navarro](https://navars.xyz) *, Meghdeep Jana, 
Soonmin Hwang, Jonathan Francis and [Jean Oh](https://cmubig.github.io/team/jean_oh/) 

*Denotes equal contribution 

<a class="button" itemprop="github" href="https://github.com/cmubig/SafeShift" target="_blank">
  <i class="fab fa-github fa-lg"></i>
</a>
<a class="button" itemprop="paper" href="https://arxiv.org/pdf/2309.08889" target="_blank">
  <i class="fas fa-file fa-lg"></i>    
</a>

# Abstract

As autonomous driving technology matures, the safety and robustness of its key components, including trajectory prediction is vital. Although real-world datasets such as Waymo Open Motion provide 
recorded real scenarios, the majority of the scenes appear benign, often lacking diverse 
safety-critical situations that are essential for developing robust models against nuanced risks.
However, generating safety-critical data using simulation faces severe simulation to real gap. Using real-world environments is even less desirable due to safety risks. In this context, we propose an approach to utilize existing real-world datasets by identifying safety relevant scenarios naively overlooked, e.g., near misses and proactive maneuvers. Our approach expands the spectrum of safety-relevance, allowing us to study trajectory prediction models under a safety-informed, 
distribution shift setting. We contribute a versatile scenario characterization method, a novel 
scoring scheme for reevaluating a scene using counterfactual scenarios to find hidden risky 
scenarios, and an evaluation of trajectory prediction models in this setting. We further contribute 
a remediation strategy, achieving a 10% average reduction in predicted trajectories’ collision rates. 

<hr>

# Method

### Overview

Scenarios sampled from real-world datasets are often deemed benign, generally lacking diverse safety-critical situations essential for developing robust models for trajectory prediction. 

Thus, existing approaches for validating autonomous driving (AD) agents have resorted to:
* **On-road tests**, where valuable rare events are potentially dangerous to other drivers and vulnerable road users. 
* **Simulated experiments**, where artificial, inaccurate behaviors can leave models unprepared for real-world deployment.  
* **Data-driven scenario generation**, where generating realistic and challenging scenarios remains an open problem.

### Motivating Example

An effective and under-explored alternative lies somewhere in the middle: **mining large-scale real-world datasets to find and leverage meaningful safety-relevant scenarios that may be hidden in the data**. 

Our key insight is that safety-relevance includes not just scenarios where observed agents act in a safety-critical manner, **but also scenarios where agents are able to avoid infractions through proactive maneuvers**. 

A motivating example is shown in the figure below, where the yellow vehicle makes an aggressive lane change in front of another vehicle. Here, we showcase two possible outcomes:
* A **safe outcome**, where the red vehicle proactively slows down to avoid a collision with the yellow vehicle, and;
* An **unsafe outcome**, where the red vehicle was distracted, and thus, fails to anticipate the yellow vehicle's lane change. 

<p align="center">
  <img width="1280" src="/assets/posts/2024-05-13-safeshift/example.png" alt="SafeShift">
</p>

### Our idea
We propose **SafeShift**, an framework consisting of: 
1. A **scenario characterization** approach focused on capturing safety-relevant scenarios naively overlooked in real-world datasets, e.g., near collisions and proactive maneuvers. 
2. A methodology for **scoring safety-criticality** which utilizes the scenario features in 1 via counterfactual probing to characterize <i>what-if</i> situations.
3. Two **downstream applications**: crafting a safety-informed distribution shift and improving the robustness of trajectory prediction models. 

<p align="center">
  <img width="1280" src="/assets/posts/2024-05-13-safeshift/model.png" alt="SafeShift">
</p>

<hr>

# Method

### Scenario Characterization 

We propose a **scenario characterization** scheme, where low-level features are computed within a scenario and then aggregated to form a score to represent a scenario’s overall safety-relevance. We consider features across two categories:

* **Individual features** which characterize single-agent state and behavior, including:
  * Features derived from positional data: speed, acceleation and jerk;
  * Context-dependent features: waiting period at a conflict point (e.g., waiting at intersections and stop signs), agent speed difference with lane's limit speed, and adherence to a lane, and;
  * Behavior features: trajectory anomalies. 
* **Social features** which characterize agent-to-agent interactions:
  * Time-to-conflict features: time headway (THW), time-to-collision (TTC), deceleration rate to avoid a crash (DRAC) and minimum time to conflict point (mTTCP);
  * Collision features: collision rate, segement-to-segment overlap, and;
  * Behavior features: trajectory-pair anomalies.  

We perform a feature correlation analysis, showing that our selected features are largely complementary,
without excessive overlap in coverage.
<p align="center">
  <img width="1100" src="/assets/posts/2024-05-13-safeshift/corrmats.png" alt="CorrMats">
</p>

### Scenario Scoring

We linearly weigh the above features to compute **individual** and **social** scores for a given agent.

<p align="center">
  <img width="1000" src="/assets/posts/2024-05-13-safeshift/indsoc_scores.png" alt="Scores">
</p>

These scores are then combined to form an overall **trajectory score**, and finally a scene score representing the density-normalized sum of all trajectory scores.
<p align="center">
  <img width="500" src="/assets/posts/2024-05-13-safeshift/traj_scores.png" alt="Scores">
</p>

We perform **counterfactual extrapolation and re-scoring** by emulating agents maintaining forward progress in their lanes, without reactivity (e.g., representing the behavior of a distracted driver).
These **future extrapolated** trajectories are combined with ground truth trajectories as follows, resulting in the desired long tail distribution 

<p align="center">
  <img width="600" src="/assets/posts/2024-05-13-safeshift/scoring_variations.png" alt="Scores">
  <img width="450" src="/assets/posts/2024-05-13-safeshift/score_densities.png" alt="Scores">
</p>

Below, we show an animated example of a counterfactual future extrapolation. Here, the agent of interest is colored in black. On the **left**, we show the **original** scene where the agent anticipates the red traffic light ahead, and thus, reduces speed. On the **right**, we show the **counterfactual** extrapolation where the agent maintains speed, failing to anticipate the traffic light, and thus, collides with the agent ahead.  

<p align="center">
  <img width="1100" src="/assets/posts/2024-05-13-safeshift/counterfactual.gif" alt="Scores">
</p>

### Downstream Tasks

We showcase our characterization and scoring methods on two downstream tasks, performed on the Waymo Open Motion Dataset (WOMD): distribution shift creation and robust trajectory prediction. 

#### Distribution Shift Creation 

We split the dataset into an **in-distribution** (ID) set comprising scenarios with low scores, and an **out-of-distribution** (OOD) set comprising the 20% highest scored scenarios.  

<p align="center">
  <img width="1280" src="/assets/posts/2024-05-13-safeshift/shift.png" alt="Scores">
</p>

#### Robust Trajectory Prediction

We propose a **remediation strategy** to improve model performance in the OOD scenes. The strategy incorporates the scenario scores into the model’s loss function to increase the prediction model performance: 

<p align="center">
  <img width="500" src="/assets/posts/2024-05-13-safeshift/score_loss.png" alt="Scores">
</p>

This loss encourages the model to not treat all scenarios and agents' trajectories as equal and to care about safety-relevant situations. 

<hr>

# Experiments and Results

#### Distribution Shift Creation 

Animated examples of low-scored scenes, where lower interactivity and less conflict regions appear in the scene. 
<p align="center">
  <img width="400" src="/assets/posts/2024-05-13-safeshift/score-1.646.gif" alt="Scores">
  <img width="400" src="/assets/posts/2024-05-13-safeshift/score-8.414.gif" alt="Scores">
</p>

Animated examples of high-scored scenes, where more complex interactions and manuevers, higher agent density and diversity, and more conflict regions are observed.
<p align="center">
  <img width="400" src="/assets/posts/2024-05-13-safeshift/score-34.579.gif" alt="Scores">
  <img width="400" src="/assets/posts/2024-05-13-safeshift/score-103.49.gif" alt="Scores">
</p>

#### Distribution Shift Results

We compare our data splitting approach (Scoring) against a random splitting approach (Uniform) and a cluster-based domain normalization approach (Clusters). 

Here, we show that our method **incurs a higher collision rate** compared to splitting methods that do not focus on safety. 

<p align="center">
  <img width="800" src="/assets/posts/2024-05-13-safeshift/shift_results.png" alt="Scores">
</p>


#### Remediation Results

We compare our remediation approach against an unremediated baseline and Frenet+ (F+), a strategy that conditions prediction models on information projected to a Frenet frame. We show that our remediation strategy was the most effective in reducing collision rates on the tested models, achieving an average 10% reduction. 

<p align="center">
  <img width="800" src="/assets/posts/2024-05-13-safeshift/rem_results.png" alt="Scores">
</p>

Qualitatively, we show that the unremediated model and the F+ strategy result in future modes that collide with an external agent. Meanwhile, our remediation approach avoids collisions, while still having reasonable mode diversity and lane conformance.

<p align="center">
  <img width="1100" src="/assets/posts/2024-05-13-safeshift/remediation_results.png" alt="Scores">
</p>

<hr>

# BibTeX

```
@INPROCEEDINGS{stoler2024safeshift,
  author={Stoler, Benjamin and Navarro, Ingrid and Jana, Meghdeep and Hwang, Soonmin and Francis, Jonathan and Oh, Jean},
  booktitle={2024 IEEE Intelligent Vehicles Symposium (IV)}, 
  title={SafeShift: Safety-Informed Distribution Shifts for Robust Trajectory Prediction in Autonomous Driving}, 
  year={2024},
  volume={},
  number={},
  pages={1179-1186},
  keywords={Meters;Collaboration;Predictive models;Robustness;Iron;Trajectory;Safety},
  doi={10.1109/IV55156.2024.10588828}}
```