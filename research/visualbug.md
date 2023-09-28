![]({{ site.url }}{{ site.baseurl }}/images/respic/visualbug/banner.png){: style="width: 1000px; height:300px; float: center;"}

### Automating Visual Bug Detection in Video Games
##### By Wei Huang | Applied Research Scientist II, Faliu Yi | Senior Applied Research Scientist


### Overview
<div style="text-align: justify">

Visual quality in video games is one of the key drivers of customer satisfaction among our gamers. Stellar graphics are essential to deliver a positive, immersive player experience, particularly in AAA titles. According to recent surveys, visual bugs are among the top 5 drivers of customer satisfaction in video games. As modern games are becoming orders of magnitude larger and more complex, this drives up the combinatorial possibilities of how bugs can manifest as well as the combinatorial enumeration of how many things needs to tested before release. Historically, the way games have been tested in the gaming industry has been manual, which runs into inherent scalability limits. Coupled with Xbox Game Studio move towards games-as-a-service, where our games are no longer a one-and-done deal, but comes with a commitment to support them for years after their release, ensuring visual quality at scale has become increasingly challenging with time. 

<p align="center">
![]({{ site.url }}{{ site.baseurl }}/images/respic/visualbug/overview_current.png){: style="width: 600px; float: center;margin-right: 30px; border: 10px"}
</p>

In recent years, computer vision(CV) has gained great achievements and success in many chalenging vision tasks. Computer vision techniques allow visual bugs to be found automatically. We are a research team at Xbox Games Studios who are leveraging computer vision techniques to visual bug detection, which transform how game studios deliver quality games to their players. 

### Potential benefits of using CV to find bugs

<ol>
<li>
 CV approach is scalable and can allow 24/7 operations
</li>
<li>
 Using CV to automatically find bugs is faster and much cheaper comparing to human testers.
</li>
<li>
 CV can deliver comparable accuracy to humans
</li>
</ol>

### Visual Bugs
Generally, there are two types of visual bugs: single-frame bugs and multi-frame bugs. 

<p align="center">
![]({{ site.url }}{{ site.baseurl }}/images/respic/visualbug/single-multi.png){: style="width: 600px; float: center;margin-right: 30px; border: 10px"}
</p>


##### Single-frame bugs
Single-frames bugs are apparent in individual images, and there is no temporal comtext between the frames before and after. Texture bug,  floating/misplaced objects, and clipping bugs are typical single-frame bugs. 

1. Texture bug
Texture measures the visual perception of objects’ surface condition in an image, such as coarseness, regularity, roughness, etc. Two common types of object texture bugs are low resolution and stretched. 

   - low resolution
<p align="center">
![]({{ site.url }}{{ site.baseurl }}/images/respic/visualbug/lowres.png){: style="width: 600px; float: center;margin-right: 30px; border: 10px"}
</p>

   - stretched texture

<p align="center">
![]({{ site.url }}{{ site.baseurl }}/images/respic/visualbug/stretched.png){: style="width: 600px; float: center;margin-right: 30px; border: 10px"}
</p>

2. Floating object
Floating object bug is one type of misplacement bug, where the object is supposed to be grounded but it's floating.

<p align="center">
![]({{ site.url }}{{ site.baseurl }}/images/respic/visualbug/floating.png){: style="width: 600px; float: center;margin-right: 30px; border: 10px"}
</p>

3. Clipping
Clipping/colission bug happens when two objects intersect, either the player/weapon or object are set incorrectly which creates visual aberrations that would not occur in the physical world. 
<p align="center">
![]({{ site.url }}{{ site.baseurl }}/images/respic/visualbug/clipping.png){: style="width: 600px; float: center;margin-right: 30px; border: 10px"}
</p>

##### Multi-frame bugs

Multi-frame bugs are not visiable in individual images, but apparent when images are viewed in sequence. Temporal context has to be examined to identify this type of bugs. Typical multi-frame bugs are Level of Details(LOD) Pop, Z-fighting, white box occlusion, etc. 

1. Level of Details(LOD) pop

<p align="center">
![]({{ site.url }}{{ site.baseurl }}/images/respic/visualbug/lodpop-1080.mp4){: style="width: 1000px; float: center;margin-right: 30px; border: 10px"}
</p>

2. Culling pop

<p align="center">
![]({{ site.url }}{{ site.baseurl }}/images/respic/visualbug/cullingpop-1080.mp4){: style="width: 1000px; float: center;margin-right: 30px; border: 10px"}
</p>

### Unique challenges in visual bug detection in gaming
<div style="text-align: justify">

The video game world is very different from our real world, which introduces unique challenges in applying CV to detect visual bugs in gaming.

<ol>
<li>
 Lack of labeled data. ML/DL models won't train well if we don't have enough labeled data. Mannual testing, data collection and annotation is slow and expensive. Transfer learning may not be able to rescue because features learned from real world datases may not be useful for many visual bugs in gaming world. 
</li>
<li>
Class imbalance. On the frame-by-frame basis, bugs are extremely rare. Generally, bugs are present in <0.1% of frames. In this case, precision must be astronomically high to avoid massive amounts of false positives. 
</li>
<li>
Highly variable distribution. Video games exhibit extreme variability in distribution. Any two games generally have very different visual features, even within same genre. A single game can vary greatly between map, game modes, etc.

<p align="center">
![]({{ site.url }}{{ site.baseurl }}/images/respic/visualbug/intro_challenge3.png){: style="width: 1000px; float: center;margin-right: 30px; border: 10px"}
</p>

</li>
</ol>

To address these challenges, we developed a synthetic data generator which allows to easily and quickly get large amounts of labeled datases. These datasets enables us to develop game-specific pre-trained models which are scalable and efficient. 

</div>

### Methodology

##### Single-frame visual bug detection
Several research works have been conducted on detecting visual bugs in video games. Rendered texture gliches are detected using deep concolution neural network ([1](https://cdn.aaai.org/ojs/7409/7409-52-10702-1-2-20200921.pdf)). A proof of concept study presents a machine learning approach for
automated detection of graphics corruptions in video games ([2](https://arxiv.org/pdf/2011.15103.pdf)). We have one published work which uses deep learning approaches to detect single-frame bugs such as clipping bug detection ([3](https://arxiv.org/abs/2309.11077))

##### Multi-frame visual bug detection
Multi-frame bugs cannot be visually recognized in individual image because they are visually correct in a single frame. These bugs can only be detected when images are viewed in sequence. Therefore, it is necessary to apply multi-frame or video-based visual bug detection methods. This means that the video-based visual bugs approach should take in either a video or multiple frames for analysis. Currently, there are few studies on video-based bug detection in the video game domain. In ([4](https://arxiv.org/abs/2208.12674v1)), a supervised method was developed to detect LOD pop, but the study only took two frames as input and required labeling for algorithm training. Several reasons may limit the development of video-based visual bug studies. Firstly, obtaining sufficient labeled data for multi-frame based visual bugs is challenging. Secondly, video games exhibit extreme variability in distribution, even within a single game, let alone across different games. Additionally, the gap between existing public video datasets and video game datasets is significant, which limits the performance of transfer learning. Despite these challenges and limited research on video-based visual bug detection, there are still potential approaches to achieve video-based visual bug detection in video games. One way is to view video-based visual bug detection as a video anomaly detection problem([5](https://arxiv.org/abs/1712.09867), [6](https://www.sciencedirect.com/science/article/abs/pii/S0262885620302109 ), [7](https://arxiv.org/abs/2009.14146)). In this case, some frame reconstruction and prediction techniques in the video anomaly detection field can be used for video-based visual bug detection. Another way is to use current popular self-supervised learning methods ([8](https://arxiv.org/pdf/2206.08356.pdf), [9](https://arxiv.org/abs/2207.00419)) to learn spatio-temporal features for each frame or video clip and fine-tune algorithm based on limited video visual bug datasets for image-level or clip-level classification or detection. Other potential methods, such as object tracking in videos ([10](https://arxiv.org/abs/2304.11968)), may also work for multi-frame visual bugs, such as culling pop bugs. Investigating these existing methods for video-based visual bug detection while researching new approaches that work for visual bugs in the field of video games is worth considering. 


### Case Study

##### LOD pop detection
In this section, we will explore some of the work that has been don to apply ML/DL to automate the detection of LOD pop bug in video game. One example of a LOD pop has been shown previously. An LOD pop bug occurs when the player can observe a sudden appearance, disappearance, or change in resolutin of an in-game object. LOD pops are not only disruptive for the player, but they are also difficult to detect using ML/DL models. The key technical challenges include:
<ol>
<li>
 Pops are highly transient, disappears after several frames, and persist for a few seconds at most.
</li>
 <li>
 Pops are not visually consistent, because any and every object in the game could cause a pop.
</li>
 <li>
 Pops usually only affect a part of the game and are highly localized.
</li>
</ol>

To detect LOD pops, we started with the standard Faster-RCNN object detection architecture, as this allows us to localize on the specific object that is causing a pop on the screen. To improve model performance, we modified the architecture to account for the temporal nature of pops. While the model’s accuracy may vary depending on the game title, we have observed that our models consistently achieve >0.8 precision and recall metrics on the held-out balanced test set. To date, our LOD pop models have been tested extensively across several AAA games. In mature deployment, ML models have contributed up to 70% of the LOD pops filed for the entire game, with over 1000+ ML-identified bug instances fixed by our game developers.



### Experimental Setup

### Results and Discussion

### Conclusion

