![]({{ site.url }}{{ site.baseurl }}/images/respic/visualbug/banner.png){: style="width: 1000px; height:300px; float: center;"}

### Automating Visual Bug Detection in Video Games
##### By Wei Huang | Applied Research Scientist


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

### Example of visual bugs
Generally, there are two types of visual bugs: single-frame bugs and multi-frame bugs. 
Single-frames bugs are apparent in individual images, and there is no temporal comtext between the frames before and after. Texture seams and clipping bugs are typical single-frame bugs. 
Multi-frame bugs are not visiable in individual images, but apparent when images are viewed in sequence. Temporal context has to be examined to identify this type of bugs. Typical multi-frame bugs are Level of Details(LOD) Pop, Z-fighting, white box occlusion, etc. 

##### Texture Seam

<p align="center">
![]({{ site.url }}{{ site.baseurl }}/images/respic/visualbug/intro_seam.png){: style="width: 600px; float: center;margin-right: 30px; border: 10px"}
</p>

##### Level of Details(LOD) Pop

<p align="center">
![]({{ site.url }}{{ site.baseurl }}/images/respic/visualbug/intro_lod.png){: style="width: 1000px; float: center;margin-right: 30px; border: 10px"}
</p>

##### Clipping


### Unique challenges in Visual Bug Detection in Gaming
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

### Case Study

##### LOD Pop Detection
In this section, we will explore some of the work that has been don to apply ML/DL to automate the detection of LOD pop bug in video game. One example of a LOD pop in Gears 5 has been shown previously. An LOD pop bug occurs when the player can observe a sudden appearance, disappearance, or change in resolutin of an in-game object. LOD pops are not only disruptive for the player, but they are also difficult to detect using ML/DL models. The key technical challenges include:
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

### Methodology

### Experimental Setup

### Results and Discussion

### Conclusion

