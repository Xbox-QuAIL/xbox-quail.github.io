![]({{ site.url }}{{ site.baseurl }}/images/respic/weak_sup/c4.png){: style="width: 1000px; height:300px; float: center;"}

### Weak Supervision for Label Efficient Visual Bug Detection
##### By Farrukh Rahman | Applied Research Scientist


### Introduction

<div style="text-align: justify">
The ever-evolving realm of video games demands a constant pursuit of visual quality and fidelity. Given the sprawling, intricate worlds now commonplace in modern games, traditional testing methods do not scale at the same rate making it an uphill task to address the myriad of potential visual discrepancies.  Machine Learning (ML) and Computer Vision (CV) methods can supplement these methods to offer scalable solutions. Reference our introduction to visual bugs [here]({{ site.baseurl }}{% link research/visualbug.md %}).
</div>

### The Challenge

<div style="text-align: justify">
Developing extensive labeled datasets pivotal for the success of deep learning is impractical for individual games due to the manual effort involved in capturing and labeling visual bugs at scale. While access to source code and the game engine does provide greater control over data diversity and availability, the evolving nature of games mandates a dynamic set of data for every new asset or environment. Moreover, game developers have narrow windows where content is testable prior to release. These factors make adaptation of our models a critical aspect in the game development cycle. However what is readily and cheaply available are unlabeled playthroughs from humans, or automated methods.

#### Can we, given an unlabeled playthrough video, use that in some manner to accelerate the development of visual bug detection models?
</div>




### Proposed Solution


<div style="text-align: justify">

Addressing the challenge, in our work we propose the use of unlabeled gameplay along with domain-specific augmentation techniques to generate datasets and self-supervised objectives. These can be used as pre-training or multi-task along with a very small amount of labeled data (order of 10s or 100s of visual bugs) enhancing the performance of the model. Our
methodology uses weak-supervision to scale datasets for the crafted objectives and fa-
cilitates both autonomous and interactive weak-supervision, incorporating unsupervised
clustering and/or an interactive approach based on text and geometric prompts.
</div>



### Method

General overview of our method: 
<p align="center">
 ![]({{ site.url }}{{ site.baseurl }}/images/respic/weak_sup/method1.png){: style="width: 600px; float: center;margin-right: 30px; border: 10px"}
</p>

- **Segmentation Stage:** Given unlabeled gameplay video, we apply a geometric promptable segmentation model (SAM) to automatically extract masks. \textbf{2. Filtering Stage:} The obtained masks are then filtered either in an unsupervised manner and/or optionally via text-interactive filtering using text-image model (CLIP). \textbf{3. Augmentation Stage:} Labeled \textit{`good'} target instances, and/or unlabeled target instances, are augmented using the filtered masks producing samples used to train a surrogate objective.
Our method involves a multi-stage approach for processing gameplay videos with the goal of augmenting visual bugs. Below is a concise summary of each stage of the method:

#### **1. Segmentation Stage:**
- **Objective:** 
   Utilizes a pre-trained promptable segmentation model to segment an unlabeled gameplay video.
   
- **Implementation:**
   Recent large pretrained segmentation models are used to perform automatic or user guided geometric prompts. Points are placed uniformly across the image in the absence of a prompt, representing the automatic/zero-shot segmentation prompt. Priors can be used to guide SAM in segmentation.

#### **2. Filtering Stage:**
- **Objective:**
   Filter and deduplicate semantic visual features omnipresent in scenes. Eg. trees, walking trails, or grass, prevalent in the outdoor park setting of our environment Giantmap. The final set of masks represents the semantics of a target game captured in unsupervised playthroughs, making them suitable candidates for visual bug augmentation.

- **Implementation:**
   Used pretrained image-text models to extract embeddings of each masked region for both autonomous and interactive filtering. Autonomous filtering involves clustering embeddings and resampling masks from each cluster to balance distribution. Interactive filtering allows users to select or reject certain masks using human guided text prompts before clustering. T


#### **3. Augmentation Stage:**
- **Objective:**
   Create a self-supervised objective through domain-specific augmentation using masks and target images. The method is flexible, allowing the source and target image to be identical in certain scenarios and can be applied across various visual bug types.

- **Implementation:**
   For first person player clipping, positives are created by overlaying the source mask *over* the target image's weapon area, while negatives are positioned *behind* the weapon, respecting the target weapon mask. Classifying positives vs negatives serve as our self-supervised objective for FPPC

### Setting
We used the same GiantMap environment as in our work [here]({{ site.baseurl }}{% link research/ai_agents_shooter22.md %}) however add 46 additional objects of interest (OOI). Particularly focusing on first-person player clipping (FPPC), where aberrations occur due to incorrectly set collision meshes. We seek to characterize how far in-distribution performance can be advanced with constrained data and assess the model’s efficacy in out-of-distribution (OOD), low-prevalence scenarios.


Additional assets added to our GiantMap environment:
<p align="center">
 ![]({{ site.url }}{{ site.baseurl }}/images/respic/weak_sup/Gm50AdditionalObj.png){: style="width: 600px; float: center;margin-right: 30px; border: 10px"}
</p>

### Results and Applications:

<div style="text-align: justify">

The proposed methodology, when applied to detect first-person player clipping/collision bugs in the expansive Giantmap game world, showcased substantial effectiveness, improving over a strong supervised baseline and capturing enough signal to outperform low-labeled supervised settings. 

<p align="center">
 ![]({{ site.url }}{{ site.baseurl }}/images/respic/weak_sup/mainresult.png){: style="width: 600px; float: center;margin-right: 30px; border: 10px"}
</p>


Our clustering step is able to capture indistribution and out-of-distribution objects from multi-view perspectives:
<p align="center">
 ![]({{ site.url }}{{ site.baseurl }}/images/respic/weak_sup/clustering.png){: style="width: 600px; float: center;margin-right: 30px; border: 10px"}
</p>

Our findings harness the potential of large-pretrained visual models to enhance our training data. Our approach allows for the injection of priors through prompting, both geometric and text-based. A significant advantage of promptable filtering is its simplicity, making it accessible for non-ML professionals, allowing them to integrate their expert knowledge into the self-supervised objective. Our pipeline's flexiblity allows it to be adapted across various visual bugs, and suggests its broad applicability in curating datasets for numerous image and video tasks within video games beyond just visual bugs. 
</div>



### Further Reading
For further details on our methods and results reference our work ([paper](https://arxiv.org/abs/2309.11077))


#### Citation


`@misc{rahman2023weak,
      title={Weak Supervision for Label Efficient Visual Bug Detection}, 
      author={Farrukh Rahman},
      year={2023},
      eprint={2309.11077},
      archivePrefix={arXiv},
      primaryClass={cs.CV}
      }` 