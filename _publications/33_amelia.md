---
layout: default
title: "Amelia: A Large Model and Dataset for Airport Surface Movement Forecasting"
blogpost_link: /amelia/
paper_url: https://arxiv.org/pdf/2407.21185
poster: null
code: https://github.com/AmeliaCMU
video: null
thumbnail: assets/img/publications/amelia.gif
authors: <b class="text-primary">Ingrid Navarro*</b>, Pablo Ortega-Kral*, Jay Patrikar*, Haichuan Wang, Zelin Ye, Jong Hoon Park, Jean Oh‡ and Sebastian Scherer‡
note: "* Equal contribution; ‡Equal Advising"
where: AIAA Aviation Forum, 2024; <b class="text-primary">Best Student Paper Award.</b>
id: paper_amelia
abstract: "
The growing demand for air travel requires technological advancements in air traffic
management as well as mechanisms for monitoring and ensuring safe and efficient operations.
In terminal airspaces, predictive models of future movements and traffic flows can help
with proactive planning and efficient coordination; however, varying airport topologies, and
interactions with other agents, among other factors, make accurate predictions challenging. Datadriven predictive models have shown promise for handling numerous variables to enable various
downstream tasks, including collision risk assessment, taxi-out time prediction, departure
metering, and emission estimations. While data-driven methods have shown improvements in
these tasks, prior works lack large-scale curated surface movement datasets within the public
domain and the development of generalizable trajectory forecasting models. In response to this,
we propose two contributions: (1) Amelia-48 dataset, a large surface movement dataset collected
using the System Wide Information Management (SWIM) Surface Movement Event Service
(SMES). With data collection beginning in December 2022, the Phase1 Amelia-48 dataset
provides more than a year’s worth of SMES data (∼30TB) and covers 48 airports within the
US National Airspace System. In addition to releasing this data in the public domain, we also
provide post-processing scripts and associated airport maps to enable research in the forecasting
domain and beyond. (2) Amelia-TF model, a transformer-based next-token-prediction large
multi-agent multi-airport trajectory forecasting model trained on 292 days or 9.4 billion tokens
of position data encompassing 10 different airports with varying topology. The open-sourced
Amelia-TF model is validated on unseen airports with experiments showcasing the different
prediction horizon lengths, ego-agent selection strategies, and training recipes to demonstrate
the generalization capabilities.
"
---