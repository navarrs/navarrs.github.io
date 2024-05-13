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
However, generating safety-critical data using simulation faces severe simulation to real gap. Using real-world environments is even less desirable due to safety risks. In this context, we propose an approach to utilize existing real-world datasets by identifying safetyrelevant scenarios naively overlooked, e.g., near misses and proactive maneuvers. Our approach expands the spectrum of safety-relevance, allowing us to study trajectory prediction models under a safety-informed, 
distribution shift setting. We contribute a versatile scenario characterization method, a novel 
scoring scheme for reevaluating a scene using counterfactual scenarios to find hidden risky 
scenarios, and an evaluation of trajectory prediction models in this setting. We further contribute 
a remediation strategy, achieving a 10% average reduction in predicted trajectories’ collision rates. 

<hr>

# Overview

Scenarios sampled from real-world datasets are often deemed benign, generally lacking diverse safety-critical situations essential for developing robust models for trajectory prediction. 

Thus, existing approaches for validating autonomous driving (AD) agents have resorted to:
* **On-road tests**, where valuable rare events are potentially dangerous to other drivers and vulnerable road users. 
* **Simulated experiments**, where artificial, inaccurate behaviors can leave models unprepared for real-world deployment.  
* **Data-driven scenario generation**, where generating realistic and challenging scenarios remains an open problem.

#### Our idea
We propose **SafeShift**, an framework consisting of: 
1. A **scenario characterization** approach focused on capturing safety-relevant scenarios naively overlooked in real-world datasets, e.g., near collisions and proactive maneuvers. 
2. A methodology for **scoring safety-criticality** which utilizes the scenario features in 1.
3. Two **downstream applications**: crafting a safety-informed distribution shift and improving the robustness of trajectory prediction models. 

<p align="center">
  <img width="1280" src="/assets/posts/2024-05-13-safeshift/model.png" alt="SafeShift">
</p>

<hr>

# Method

<hr>

# Experiments and Results

<hr>


# BibTeX

```
@article{stoler2023safeshift,
  title={SafeShift: Safety-Informed Distribution Shifts for Robust Trajectory Prediction in Autonomous Driving},
  author={Stoler, Benjamin and Navarro, Ingrid and Jana, Meghdeep and Hwang, Soonmin and Francis, Jonathan and Oh, Jean},
  journal={arXiv preprint arXiv:2309.08889},
  year={2023}
}
```