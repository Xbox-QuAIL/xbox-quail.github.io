![]({{ site.url }}{{ site.baseurl }}/images/respic/aiagent_demo22/banner.png){: style="width: 1000px; height:300px; float: center;"}

### AI Agents for Open-world Game Exploration and Testing 
##### By Sherif Gad | Senior Applied Research Scientist


### Overview
<div style="text-align: justify">

The gaming industry is one of the giant IT sectors with revenue projections to reach $257bn in 2023 ([1](https://www.weforum.org/agenda/2022/07/gaming-pandemic-lockdowns-pwc-growth/)). With an increasing demand from a growing consumer base, especially after the pandemic, game developers are facing a severe challenge in committing to strict release deadlines while preserving high quality for shipped games. To overcome such a challenge, gaming studios depend on large teams of game quality control engineers, significantly increasing operating costs and adding a non-trivial burden to manage work across such large groups. While effective, this approach will not scale in terms of cost and time, given the increasing demand and game complexity. 

Over the last decade, some automation attempts aimed at addressing this dilemma, such as test automation tools that facilitate writing deterministic workflows for test cases and executing them using integration APIs with game engines. Nevertheless, due to their inflexible hard-coded workflows, such tools can not adapt or scale sufficiently across different test scenarios and games.

AI agents that can learn from interaction with the environment and demonstrations by humans provide a practical toolkit that can scale across different games with minimum dependencies on specific scenarios. Reinforcement learning (RL) ([2](https://lilianweng.github.io/posts/2018-02-19-rl-overview/)) is one of the most utilized learning paradigms for training AI agents in a closed-loop control workflow involving sensing the environment's state, taking the best action based on agent's experience, and incurring a reward signal indicating how good was the action taken from a task perspective. RL methods have achieved many breakthroughs in playing games with human-level performance ([3](https://ai.googleblog.com/2015/02/from-pixels-to-actions-human-level.html?m=1/))([4](https://www.deepmind.com/blog/agent57-outperforming-the-human-atari-benchmark)).
</div>

<p align="center">
![]({{ site.url }}{{ site.baseurl }}/images/respic/aiagent_demo22/RL_intro.png){: style="width: 600px; float: center;margin-right: 30px; border: 10px"}
</p>

### Motivation
<div style="text-align: justify">

We got motivated by two major limitations in the current methods using AI agents for gaming testing, including:

<ol>
<li>
 Dependency on the game state (e.g., 3D semantic maps or enemies' positions) in the state representation. This dependency requires integration efforts from the game developers and limits the generalization across games. To resolve
</li>
<li>
Usually, AI agents for game testing maximize a novelty-seeking objective (e.g., state visitation counts) for game exploration. Using such an approach in open-world games is sample inefficient (i.e., it requires many trials to converge). Moreover, it lacks an explicit way to condition the agent's exploration behavior according to a user's preference (e.g., following the golden path). 
</li>
</ol>

To address these limitations, we developed a novel agent design that only depends on pixel-based observations (i.e., RGB frames) with the ability to condition its exploration behavior based on a human preference introduced in the form of demonstrations. 
</div>

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
For further details about our PPGTA agent, please refer to our research ([paper](https://arxiv.org/abs/2308.09289)) that we recently presented at the IEEE Conference on Games (CoG) 2023.

### Cite
`@article{ppgta_2023,
  author       = {Sherif Abdelfattah and
                  Adrian Brown and
                  Pushi Zhang},
  title        = {Preference-conditioned Pixel-based {AI} Agent For Game Testing},
  journal      = {CoRR},
  volume       = {abs/2308.09289},
  year         = {2023},
  url          = {https://doi.org/10.48550/arXiv.2308.09289},
  doi          = {10.48550/arXiv.2308.09289},
  eprinttype    = {arXiv},
  eprint       = {2308.09289}
}`
