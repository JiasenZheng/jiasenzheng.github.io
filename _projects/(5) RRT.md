---
name:  Rapidly-exploring Random Tree
tools: [RRT, Python, Path Planning]
image: https://jiasenzheng.github.io/assets/RRT3.gif
description: Implementations of the RRT algorithm under three different conditions
---

# Rapidly-exploring Random Tree <br><br>

### Brief overview
A rapidly-explore random tree (RRT) algorithm was developed by `Steven M. LaValle` and `James J. Kuffner Jr.` to search an environment effectively. A search tree is generated incrementally from a starting position and then randomly grows towards the unsearched area of the map. Three environmental conditions will be presented in this project.

### Results
* RRT with no obstacles (100 & 1000 iterations)
<img src="{{ site.url }}{{ site.baseurl }}/assets/RRT0.gif" />
<img src="{{ site.url }}{{ site.baseurl }}/assets/RRT1.gif" />
* RRT with circular obstacles
<img src="{{ site.url }}{{ site.baseurl }}/assets/RRT2.gif" />
* RRT with arbitary obstacles
<img src="{{ site.url }}{{ site.baseurl }}/assets/RRT3.gif" />

### Algorithm descriptions
* pseudocode for the basic algorithm (no obstacle)

<img src="{{ site.url }}{{ site.baseurl }}/assets/RRT4.png" /><br>

* **RANDOM_CONFIGURATION** Generates a random position in the map
* **NEAREST_VERTEX** Finds the vertex in the G-list that is closest to the given position
* **NEW_CONFIGURATION** Generates a configuration by moving a distance from the nearest vertex towards the random position
* **CHECK_POINT_COLLISION** Checks if the point collides with the obstacles
* **CHECK_EDGE_COLLISION** Checks if the edge collides with the obstacles
* **CHECK_COLLISION_FREE_PATH** Check if there is a collision-free path to the goal position <br><br>

<p class="text-center">
{% include elements/button.html link="https://github.com/JiasenZheng/Rapidly-Exploring-Random-Tree" text="GitHub" %}
</p>
