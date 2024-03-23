---
layout: default
title: "Knowledge-driven Scene Priors for Semantic Audio-Visual Embodied Navigation"
paper_url: https://arxiv.org/pdf/2212.11345.pdf
poster: null
code: null
video: null
thumbnail: assets/img/publications/ksaven.gif
authors: Gyan Tatiya, Jonathan Francis, Luca Bondi, <b class="text-primary">Ingrid Navarro</b> Eric Nyberg, Jivko Sinapov, and Jean Oh
note: null
where: Preprint in ArXiv, 2022
id: paper_ksaven
abstract: "
Generalisation to unseen contexts remains a challenge for embodied navigation agents. In the context 
of semantic audio-visual navigation (SAVi) tasks, the notion of generalisation should include both 
generalising to unseen indoor visual scenes as well as generalising to unheard sounding objects. 
However, previous SAVi task definitions do not include evaluation conditions on truly novel sounding 
objects, resorting instead to evaluating agents on unheard sound clips of known objects; meanwhile, 
previous SAVi methods do not include explicit mechanisms for incorporating domain knowledge about 
object and region semantics. These weaknesses limit the development and assessment of models' 
abilities to generalise their learned experience. In this work, we introduce the use of 
knowledge-driven scene priors in the semantic audio-visual embodied navigation task: we combine 
semantic information from our novel knowledge graph that encodes object-region relations, spatial 
knowledge from dual Graph Encoder Networks, and background knowledge from a series of pre-training tasks 
-- all within a reinforcement learning framework for audio-visual navigation. We also define a new 
audio-visual navigation sub-task, where agents are evaluated on novel sounding objects, as opposed 
to unheard clips of known objects. We show improvements over strong baselines in generalisation to 
unseen regions and novel sounding objects, within the Habitat-Matterport3D simulation environment, 
under the SoundSpaces task. 
"
---