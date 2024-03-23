---
layout: default
title: "Social-PatteRNN: Socially-Aware Trajectory Prediction Guided by Motion Patterns"
paper_url: https://arxiv.org/pdf/2209.05649.pdf
poster: null
code: https://github.com/cmubig/social-patternn
video: null
thumbnail: assets/img/publications/sprnn.gif
authors: <b class="text-primary">Ingrid Navarro</b> and Jean Oh
note: null
where: IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS), 2022
id: paper_sprnn
abstract: "
As intelligent robots across domains start collaborating with humans in shared environments, 
<i>e.g.</i>, urban settings and airspace, algorithms that enable them to reason over human motion 
and intent are important to ensure seamless and safe interplay. Even beyond robotics, other domains, 
<i>e.g.</i>, surveillance and sports analysis, may also benefit from this type of algorithms. 

In our work, we study human intent by focusing on the problem of predicting trajectories in dynamic 
environments. We are further interested in designing methods that are able to generalize across 
domains. Specifically, we target domains where navigation guidelines are relatively strictly defined 
yet not necessarily marked in their physical environments. We hypothesize that within these domains, 
in the short-term, agents tend to exhibit motion patterns that reveal important context information 
related to the agent's general direction, admissible motions, intermediate goals and social 
influences. From this intuition, we propose <i>Social-PatteRNN</i>, a new recurrent generative model 
that exploits motion patterns to encode the aforesaid context information and use it as conditioning
signal for predicting trajectories. We assess our approach across three different problem domains: 
human motion in crowds, human motion in sports and aircraft motion in terminal airspace. Finally, we 
show that our approach achieves state-of-the-art results across these domains.
"
---