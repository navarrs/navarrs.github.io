---
layout: default
title: "ScenarioCharacterization: A Modular Toolkit for Characterizing Safety across Trajectory Datasets"
blogpost_link: /scenario-characterization/
paper_url: https://arxiv.org/abs/2608.16041
poster: null
code: https://github.com/navarrs/ScenarioCharacterization
video: null
thumbnail: assets/img/publications/scenariocharacterization.png
authors: <b class="text-primary">Ingrid Navarro†</b>, Yutong Duan, Jonathan Francis‡ and Jean Oh‡
note: "† Developed in part during an internship at <b>Stack AV</b>; ‡ Equal advising"
where: Preprint in ArXiv, 2026
id: paper_scenariocharacterization
abstract: "
<p>We introduce <b>ScenarioCharacterization</b>, an open-source framework for automated, dataset-agnostic
profiling of driving scenarios in trajectory datasets. Our framework is packaged as a modular,
configuration-driven pipeline of three layers: a <i>dataset adapter</i> that maps custom datasets onto an open
Scenario representation, a <i>characterizer</i> that performs feature extraction, behavior probing, and
criticality scoring at scenario and agent levels, and an <i>analysis</i> layer for scenario visualization and
feature, score, and probe analyses. </p>
<p>Because the layers communicate only through Pydantic-validated schemas composed via configurations, a new
dataset can easily plug in without rewriting the characterization and analysis stack. This technical report
describes the design and APIs, shows example outputs on Waymo Open Motion, Argoverse2, and nuPlan, and discusses
downstream uses of the approach. </p>
"
---
