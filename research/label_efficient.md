![]({{ site.url }}{{ site.baseurl }}/images/respic/weak_sup/method1.png){: style="width: 1000px; height:300px; float: center;"}

### Weak Supervision for Label Efficient Visual Bug Detection
##### By Farrukh Rahman | Applied Research Scientist


### Introduction

<div style="text-align: justify">
The ever-evolving realm of video games demands a constant pursuit of visual quality and fidelity. Given the sprawling, intricate worlds now commonplace in modern games, traditional testing methods do not scale at the same rate making it an uphill task to address the myriad of potential visual discrepancies.  Machine Learning (ML) and Computer Vision (CV) methods can supplement these methods to offer scalable solutions.
</div>

### The Challenge

<div style="text-align: justify">
Developing extensive labeled datasets pivotal for the success of deep learning is impractical for individual games due to the manual effort involved in capturing and labeling visual bugs at scale. While access to source code and the game engine does provide greater control over data diversity and availability, the evolving nature of games mandates a dynamic set of data for every new asset or environment. Moreover, game developers have narrow windows where content is testable prior to release. These factors make adaptation of our models a critical aspect in the game development cycle. However what is readily and cheaply available are unlabeled playthroughs from humans, or automated methods.

#### Can we, given an unlabeled playthrough video, use that in some manner to accelerate the development of visual bug detection models?
</div>


### Proposed Solution

<div style="text-align: justify">

Addressing the challenge in our work we propose the use of unlabeled gameplay along with domain-specific augmentation techniques to generate datasets and self-supervised objectives. These can be used as pre-training or multi-task along with a very small amount of labeled data (order of 10s or 100s of visual bugs) enhancing the performance of the model.
</div>

<p align="center">
 ![]({{ site.url }}{{ site.baseurl }}/images/respic/weak_sup/method1.png){: style="width: 600px; float: center;margin-right: 30px; border: 10px"}
</p>

The described method involves a multi-stage approach for processing gameplay videos with the goal of augmenting visual bugs, specifically in a first-person shooting game. Below is a concise summary of each stage of the method:

#### **1. Segmentation Stage:**
- **Objective:** 
   Utilizes a pre-trained segmentation model to segment different regions from an unlabeled gameplay video.
   
- **Process:**
   SAM can perform auto-prompted or geometrically prompted segmentation. Points are placed uniformly across the image in the absence of a prompt, representing the automatic/zero-shot segmentation prompt. Priors can be used to guide SAM in segmentation.

#### **2. Filtering Stage:**
- **Objective:**
   Filter and deduplicate semantic visual features like trees, walking trails, or grass, prevalent in the outdoor park setting of the game.

- **Process:**
   Uses CLIP to extract embeddings of each masked region for both autonomous and interactive filtering. Autonomous filtering involves clustering embeddings using Hierarchical Agglomerative Clustering (HAC) and resampling masks from each cluster to balance distribution. Interactive filtering allows users to select or reject certain masks using text prompts before clustering. The final set of masks represents the semantics of a target game captured in unsupervised playthroughs, making them suitable candidates for visual bug augmentation.

#### **3. Augmentation Stage:**
- **Objective:**
   Create a self-supervised objective through domain-specific augmentation using masks and target images.

- **Process:**
   Positive examples denote bugs and negative examples denote normal or no-bug situations. The method is flexible, allowing the source and target image to be identical in certain scenarios and can be applied across various visual bug types.


### Results and Applications:

The proposed methodology, when applied to detect first-person player clipping/collision bugs in the expansive Giantmap game world, showcased substantial effectiveness, improving over a strong supervised baseline and capturing enough signal to outperform low-labeled supervised settings. It also demonstrated adaptability across various visual bugs, suggesting its broad applicability in curating datasets for numerous image and video tasks within video games beyond just visual bugs.

### Previous Work

Despite the existence of previous AI agents for game testing work ([5](https://ieeexplore.ieee.org/document/8848091), [6](https://ieeexplore.ieee.org/document/8952543), [7](https://ieeexplore.ieee.org/document/9231552), [8](https://arxiv.org/abs/2103.13798), [9](https://arxiv.org/abs/2201.06865)), there were still research gaps to address as follows:

<ol>
<li>
Many of the existing AI agents depend on internal game state information (e.g., 3D semantic maps of the environment) in their state representation, which demands a non-trivial effort from game developers to provide engine integration hooks collecting such internal information. Our PPGTA agent addresses this limitation mainly by depending only on pixel-based (i.e., RGB frames) state representation.
</li>
<li>
Most exploration policies depend solely on novelty rewards without explicit ways to condition /bias the exploration behavior on a given user's preference for exploration, making them inefficient in complex game environments. Our PPGTA agent tackles this limitation by incorporating a preference enforcement module that regulates the exploration behavior based on preference style rewards. 
</li>
<li>
Utilized designs for imitation learning policies do not properly consider spatio-temporal dependencies between different semantics in the scene (e.g., assets). Our PPGTA agent utilizes a novel memory-based imitation policy with a pre-trained visual feature extractor to capture time-based relationships across semantics in the scene.
</li>
</ol>

### Game Environment

<p align="center">
 ![]({{ site.url }}{{ site.baseurl }}/images/respic/aiagent_demo22/shooter_env.png){: style="width: 600px; float: center;margin-right: 30px; border: 10px"}
</p>

<div style="text-align: justify">
We aimed to design a game scenario that resembles the complexity of real open-world AAA games. Our demo game mimics a large city park (e.g., New York Hyde Park) with a total playable are of 8.6 square kilometers, including trails, green spaces, lakes, bridges, and service buildings like cafeterias and toilets. Along the trials, there are objects to be inspected for bugs such as clipping or texture. Yet, not all objects are of interest to test. The agent's task is to explore such an open-world environment, find objects of interest, and perform bug testing on them similarly to human tester demonstrations.
</div>

<p align="center">
 ![]({{ site.url }}{{ site.baseurl }}/images/respic/aiagent_demo22/task_objectives.png){: style="width: 600px; float: center;margin-right: 30px; border: 10px"}
</p>

### Methodology

<div style="text-align: justify">

<p align="center">
 ![]({{ site.url }}{{ site.baseurl }}/images/respic/aiagent_demo22/main_framework.png){: style="width: 600px; float: center;margin-right: 30px; border: 10px"}
</p>

Our Preference-conditioned Pixel-based Game Testing Agent (PPGTA) extends on the Inspector agent consists of four main building blocks, including:

<ul>
<li> An object detector model to detect objects of interest (OOIs) during game exploration.</li>
<li> An imitation learning policy to execute bug testing based human tester demonstration datasets.</li>
<li> A novelty model for exploring the environment guided by curiosity rewards (i.e., incentifying novel scenes).</li>
<li> A preference enforcement module that regulates the exploration behavior guided by human's preference introduced by demonstrations.</li>
</ul>

</div>

<div style="text-align: justify">

</div>

### Demo

<p align="center">
<video width="600" height="400" controls>
  <source src="{{ site.url }}{{ site.baseurl }}/images/respic/aiagent_demo22/tiny_agents_demo.mp4" type="video/mp4">
</video>
</p>

### Further Reading
For further details on our methods reference our work ([paper](https://arxiv.org/abs/2309.11077))


### citation


`@misc{rahman2023weak,
      title={Weak Supervision for Label Efficient Visual Bug Detection}, 
      author={Farrukh Rahman},
      year={2023},
      eprint={2309.11077},
      archivePrefix={arXiv},
      primaryClass={cs.CV}
}` 