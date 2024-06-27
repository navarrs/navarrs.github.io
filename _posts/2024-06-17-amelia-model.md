---
layout: post
author: Ingrid Navarro
permalink: /amelia-model/
title: AmeliaTF
image: /assets/posts/2024-06-17-amelia-model/model.png
link-new-tab: true
categories: publication
# hero_image: /assets/posts/2024-06-14-amelia-dataset/klax_header.gif
hero_image: /assets/img/background.gif
hero_height: is-small
---

<h1> 
AmeliaTF: A Large Airport Surface Movement Trajectory Forecasting Model
</h1>

[Ingrid Navarro](https://navars.xyz) *, [Pablo Ortega-Kral](https://paok-2001.github.io) *, [Jay Patrikar](https://www.jaypatrikar.me) *, Haichuan Wang, 
Zelin Ye, Jong Hoon Park, [Jean Oh](https://cmubig.github.io/team/jean_oh/) and [Sebastian Scherer](https://theairlab.org/team/sebastian/) 

*Denotes equal contribution 

**This work is a collaboration between the Bot Intelligence Group ([BIG](https://cmubig.github.io)) and the [AirLab](https://theairlab.org) at Carnegie Mellon University!**

<a class="button" itemprop="paper" href="https://arxiv.org/pdf/2309.08889" target="_blank">
  <i class="fas fa-database fa-lg"></i>    
</a> 
<a class="button" itemprop="paper" href="https://arxiv.org/pdf/2309.08889" target="_blank">
  <i class="fas fa-file fa-lg"></i>    
</a> 

<hr>

# AmeliaTF

Predictive models for airport surface operations can be used for various downstream tasks like 
collision risk assessment, taxi-out time prediction, departure metering, and emission estimations. 
While data-driven methods have showcased marked improvements in predictive performance in recent 
years, prior works have not addressed the lack of large-scale curated surface movement datasets 
within the public domain and the development of generalizable trajectory forecasting models. 

In response to this, we introduce **AmeliaTF** is a large transformer-based airport surface movement 
trajectory forecasting model trained on the **[Amelia-48](https://navars.xyz/amelia-dataset)** 
dataset. We explore different scene representation and training strategies for our model varying from 
single-airport to multi-airport settings in which we assess our model's generalization capabilities.

Below, we provide an overview of our model and experiments. For more details, please check out our paper! 

## Model Overview

AmeliaTF is an end-to-end motion prediction model that aims to characterize *relevant* surface
area operations. To do so, our model comprises three main submodules: 
1. A **scene representation** module that determines the agents of interest in the scene using a scoring strategy, and encodes per-agent features, 
2. A transformer-based **scene encoder**, which hierarchically encodes the *temporal*, *agent-to-agent* and *agent-to-context* relationships within a scene, and; 
3. A **trajectory decoder** that models the set of possible futures with associated confidence scores using a Gaussian Mixture Model. 

<div align="center">
<video width="1000" autoplay loop muted>
  <source src="/assets/posts/2024-06-17-amelia-model/amelia_overview.webm" type="video/mp4" />
</video>
</div>

# Experiments and Results

We explored two main experiments to assess the performance of **AmeliaTF**. The first experiment 
studies the benefit of our proposed scene representation strategy. The second experiment explores the generalization of our model across airports. 

### Ego-selection strategy

We want to assess if our ego-selection strategy produces complex and interesting scene 
representations since it prioritizes more critical agent-to-agent relationships and more dynamic agent motion profiles within a scene. 

To do so we compare our proposed idea against a *random* agent selection strategy. 

#### Quantitative Results

<div align="center">
<video width="1000" autoplay loop muted>
  <source src="/assets/posts/2024-06-17-amelia-model/ego_results.webm" type="video/mp4" />
</video>
</div>

#### Qualitative Results

<style>
.egoSlides {display:none;}
</style>

<div align="center">
  <table align="center">
    <tr>
      <td><b>Random Ego-agent Selection Strategy</b></td>
      <td><b>Critical Ego-agent Selection Strategy</b></td>
    </tr>
  </table>
  <img class="egoSlides" src="/assets/posts/2024-06-17-amelia-model/ego_results_1.png" style="width:100%">
  <img class="egoSlides" src="/assets/posts/2024-06-17-amelia-model/ego_results_2.png" style="width:100%">
  <img class="egoSlides" src="/assets/posts/2024-06-17-amelia-model/ego_results_3.png" style="width:100%">
  <img class="egoSlides" src="/assets/posts/2024-06-17-amelia-model/ego_results_4.png" style="width:100%">
  <img class="egoSlides" src="/assets/posts/2024-06-17-amelia-model/ego_results_5.png" style="width:100%">
  
  <div align="center">
    <button class="button-slide" onclick="plusDivs(-1, 'egoSlides')">&#10094;</button>
    <button class="button-slide" onclick="plusDivs(1, 'egoSlides')">&#10095;</button>
  </div>
</div>

### Multi-Airport generalization

We also explore our model's generalization capabilities as we cover a wider variety of training data. We hypothesize that as we do so, our model will learn richer representations that would generalize better to unseen airport layouts and interactions. 

This would eventually reduce the requirement for more training data and/or adaptation techniques, such as fine-tuning. Thus, we propose a *multi-airport* ablation to assess **AmeliaTF**'s performance in *unseen* airports as we vary the number of *seen* ones during training. 


#### Quantitative Results

#### Qualitative Results

<style>
.genSlides {display:none;}
</style>

<div align="center">
  <table align="center">
    <tr>
      <td><b>Seen Airports&emsp;&emsp;</b></td>
      <td><b>Unseen Airports</b></td>
    </tr>
  </table>
  <img class="genSlides" src="/assets/posts/2024-06-17-amelia-model/ego_results_1.png" style="width:100%">
  <img class="genSlides" src="/assets/posts/2024-06-17-amelia-model/ego_results_2.png" style="width:100%">
  <img class="genSlides" src="/assets/posts/2024-06-17-amelia-model/ego_results_3.png" style="width:100%">
  <img class="genSlides" src="/assets/posts/2024-06-17-amelia-model/ego_results_4.png" style="width:100%">
  <img class="genSlides" src="/assets/posts/2024-06-17-amelia-model/ego_results_5.png" style="width:100%">
  <br> 
  <div align="center">
    <button class="button-slide" onclick="plusDivs(-1, 'genSlides')">&#10094;</button>
    <button class="button-slide" onclick="plusDivs(1, 'genSlides')">&#10095;</button>
  </div>
</div>

<script>
var slideIndex = 1;
showDivs(slideIndex, 'egoSlides');
showDivs(slideIndex, 'genSlides');

function plusDivs(n, class_name) {
  showDivs(slideIndex += n, class_name);
}

function showDivs(n, class_name) {
  var i;
  var x = document.getElementsByClassName(class_name);
  if (n > x.length) {slideIndex = 1}
  if (n < 1) {slideIndex = x.length}
  for (i = 0; i < x.length; i++) {
    x[i].style.display = "none";  
  }
  x[slideIndex-1].style.display = "block";  
}
</script>

# BibTeX

If you find our work useful in your research, please cite us!

```
@article{navarro2024amelia,
  title={Amelia: A Large Model and Dataset for Airport Surface
Movement Forecasting},
  author={Nasvarro, Ingrid and Ortega-Kral, Pablo and Patrikar, Jay, and Haichuan, Wang and Park, Jong Hoon and Oh, Jean and Scherer, Sebastian},
  journal={arXiv preprint arXiv:2309.08889},
  year={2024}
}
```