---
layout: default
title: "SoRTS: Learned Tree Search for Long-Horizon Social Robot Navigation"
paper_url: https://arxiv.org/pdf/2309.13144.pdf
poster: null
code: https://github.com/cmubig/sorts
video: https://youtu.be/PBE3O4cW2rI
thumbnail: assets/img/publications/sorts.gif
authors: <b class="text-primary">Ingrid Navarro*</b>, Jay Patrikar*, Joao P. A. Dantas, Rohan Baijal, Ian Higgins, Sebastian Scherer, and Jean Oh
note: \* Equal contribution
where: IEEE Robotics and Automation Letters, 2024
id: paper_sorts
abstract: "The fast-growing demand for fully autonomous robots in shared spaces calls for the 
development of trustworthy agents that can safely and seamlessly navigate in crowded environments. 
Recent models for motion prediction show promise in characterizing social interactions in such 
environments. Still, adapting them for navigation is challenging as they often suffer from 
generalization failures. Prompted by this, we propose <i>Social Robot Tree Search (SoRTS)</i>, an 
algorithm for safe robot navigation in social domains. SoRTS aims to augment existing socially aware 
motion prediction models for long-horizon navigation using Monte Carlo Tree Search.

We use social navigation in general aviation as a case study to evaluate our approach and further 
the research in full-scale aerial autonomy. In doing so, we introduce <i>X-PlaneROS</i>, a 
highfidelity aerial simulator that enables human-robot interaction. We use X-PlaneROS to conduct a 
first-of-its-kind user study where 26 FAA-certified pilots interact with a human pilot, our algorithm, 
and its ablation. Our results, supported by statistical evidence, show that SoRTS exhibits a comparable 
performance to competent human pilots, significantly outperforming its ablation. Finally, we complement 
these results with a broad set of self-play experiments to showcase our algorithm’s performance in 
scenarios with increasing complexity.
"
---