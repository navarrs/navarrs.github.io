---
layout: default
title: "SafeShift: Safety-Informed Distribution Shift for Robust Trajectory Prediction in Autonomous Driving"
blogpost_link: /safeshift/
paper_url: https://arxiv.org/pdf/2309.08889.pdf
poster: null
code: https://github.com/cmubig/SafeShift
video: null
thumbnail: assets/img/publications/safeshift.png
authors: Benjamin Stoler*, <b class="text-primary">Ingrid Navarro*</b>, Meghdeep Jana, Soonmin Hwang, Jonathan Francis, and Jean Oh
note: "* Equal contribution"
where: Preprint in ArXiv (Manuscript currently under review for IEEE IV 2024)
id: paper_safeshift
abstract: "
As autonomous driving technology matures, safety and robustness of its key components, including 
trajectory prediction, is vital. Though real-world datasets, such as Waymo Open Motion, provide 
realistic recorded scenarios for model development, they often lack truly safety-critical situations. 
Rather than utilizing unrealistic simulation or dangerous realworld testing, we instead propose a 
framework to characterize such datasets and find hidden safety-relevant scenarios within. Our approach 
expands the spectrum of safety-relevance, allowing us to study trajectory prediction models under 
a safety-informed, distribution shift setting. We contribute a generalized scenario characterization 
method, a novel scoring scheme to find subtly-avoided risky scenarios and an evaluation of trajectory 
prediction models in this setting. We further contribute a remediation strategy, achieving a 10% 
average reduction in prediction collision rates. 
"
---