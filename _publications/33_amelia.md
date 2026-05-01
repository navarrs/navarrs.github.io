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
<p>Demand for air travel is rising, straining existing aviation infrastructure. In the US, more than 90% of airport control towers are understaffed, falling short of FAA and union standards. This, in part, has contributed to an uptick in near-misses and safety-critical events, highlighting the need for advancements in air traffic management technologies to ensure safe and efficient operations. </p>

<p>Data-driven predictive models for terminal airspace show potential to address these challenges; however, the lack of large-scale surface movement datasets in the public domain has hindered the development of scalable and generalizable approaches.</p>

<p>To address this, we introduce <b>Amelia-42</b>, a first-of-its-kind large collection of raw airport
surface movements reports streamed through the FAA’s System Wide Information Management
(SWIM) Program, comprising over two years of trajectory data (∼9.19TB) across 42 US airports.
We open-source tools to process this data into clean tabular position reports. We release
<b>Amelia42-Mini</b>, a 15-day sample per airport, fully processed data on HuggingFace for ease
of use. We also present a trajectory forecasting benchmark consisting of <b>Amelia10-Bench</b>, an accessible experiment family using 292 days from 10 airports, as well as <b>Amelia-TF</b>, a
transformer-based baseline for multi-agent trajectory forecasting. </p>
"
---