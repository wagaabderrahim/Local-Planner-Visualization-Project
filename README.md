<div align="center">
<img src="media/2.png"/>
</div>

<div align="center">
<img src="media/1.png"/>
</div>

<div align="center">

![](https://img.shields.io/github/license/abdurj/local-planner-visualization-project)
![](https://img.shields.io/github/last-commit/abdurj/local-planner-visualization-project)
![](https://img.shields.io/github/v/release/abdurj/local-planner-visualization-project)
![](https://img.shields.io/github/stars/abdurj/local-planner-visualization-project)

</div>

# Table of Contents
- [Table of Contents](#table-of-contents)
- [Local Planner Visualization Project (LPVP)](#local-planner-visualization-project-lpvp)
- [Features](#features)
- [Installation/Usage](#installationusage)
- [Local Planners](#local-planners)
  - [Probabilistic Roadmap (PRM)](#probabilistic-roadmap-prm)
    - [1. Sampling Stage](#1-sampling-stage)
    - [2. Creating the roadmap](#2-creating-the-roadmap)
    - [3. Searching the Roadmap](#3-searching-the-roadmap)
  - [Rapidly-exploring Random Tree (RRT)](#rapidly-exploring-random-tree-rrt)
  - [Potential Field](#potential-field)
    - [Virtual Fields](#virtual-fields)
  - [Grid Based Planner](#grid-based-planner)
- [Current Issues](#current-issues)
- [Contributing](#contributing)
- [Roadmap](#roadmap)
- [Authors](#authors)
- [Acknowledgements](#acknowledgements)
- [License](#license)


# Local Planner Visualization Project (LPVP)

LPVP serves to provide a single application to visualize numerous different local planner algorithms
used in Autonomous Vehicle path planning and mobile robotics. The application 
provides customizable parameters to better understand the inner workings of each algorithm and explore their strengths and
drawbacks. It is written in python and uses Pygame to render the visualizations.

![App Preview](media/lpvp.gif)
<!-- add proper gif link -->


# Features

- Multiple Local Planner Algorithms
    - Probabilistic Roadmap
    - RRT
    - Potential Field
- Multiple Graph Search Algorithms
    - Dijkstra's Shortest Path
    - A* Search
    - Greedy Best First Search
- Graph Search visualization
- Random obstacle generation with customizable obstacle count
- Drag and drop obstacle generation
- Drag and drop customizable start/end pose
- Customizable Parameters for each planner algorithm
    - Probabilistic Roadmap
        - Sample Size
        - K-Neighbours
        - Graph Search algorithm
    - RRT
        - Path goal bias
    -  Potential Field
        - Virtual Field toggle
- Support for additional planner and search algorithms
# Installation/Usage

The project is written in Python3, and uses `pygame` to handle the visualizations and `pygame_gui` for the gui.`Numpy` is used
for calculations for the potential field planner.

1. Clone the repo
```bash
git clone https://github.com/abdurj/Local-Planner-Visualization-Project.git
```
2. Install Dependencies 
```bash
  pip3 install pygame pygame_gui Numpy
  cd Local-Planner-Visualization-Project
```
3. Run the program
```bash
python3 base.py
```
    
# Local Planners

## Probabilistic Roadmap (PRM)
The probabilistic roadmap planner is a sampling based planner that operates in 3 stages, and searches a constructed graph network to
find the path between the start and end configuration. This approach is heavy on pre-processing, as it needs to 
generate the network, however after the preprocessing is done, it can quickly search the constructed network
for any start and goal pose configuration without needing to restart. The PRM excels in solving motion planning problems in high dimensional C-Spaces, for example: a robot with many joints. However this project demonstrates a PRM acting in a 2D C-Space.


### 1. Sampling Stage
During the sampling stage the planner generates N-samples from the free C-Space. 
In this project, the samples are generated by uniformly sampling the C-Space, and if the sample
does not collide with any object, it is added as a Node in the roadmap. The PRM isn't limited to 
uniform sampling techniques, non-uniform sampling techniques can be used to better model the C-Space.

Non-uniform sampling methods are planned for a future release

![App Preview](https://i.imgur.com/4iWmjax.gif)

### 2. Creating the roadmap 
In the next stage, the planner finds the K closest neighbours for each node. It then uses a simple local path planner
to connect the node with it's neighbour nodes without trying to avoid any obstacles. This is done by simply
creating a straight line between the nodes. If this line is collision free; an edge is created between the nodes. 

![App Preview](https://i.imgur.com/8w35Gi3.gif)

### 3. Searching the Roadmap
After connecting all nodes with its K-closest neighbours, a resulting graph network will have been created.
This network can be searched with a graph search algorithm. The currently supported graph search algorithms are:
- Dijkstra's Shortest Path
- A* Search
- Greedy Best First Search
More search algorithms are planned for a future release.

![App Preview](https://i.imgur.com/xshynqi.gif)

## Rapidly-exploring Random Tree (RRT)
The rapidly-exploring random tree planner is another sampling based planner that explores the C-space by growing a tree rooted at the starting configuration.
It then randomly samples the free c-space, and attempts to connect the random sample with the nearest node in the tree.
The length of the connection is limited by a growth factor, or "step size". In path planning problems, a bias factor
is introduced into the RRT. This bias factor introduces a probability that the random sample will be the goal pose. 
Increasing the bias factor affects how greedily the tree expands towards the goal. 
![RRT](https://i.imgur.com/JxkF82B.gif)


## Potential Field
The potential field planner is adapted from the concept of a charged particle travelling through a charged magnetic field.
The goal pose emits a strong attractive force, and the obstacles emit a repulsive force. We can emulate this behaviour by
creating a artificial potential field that attracts the robot towards the goal. The goal pose emits a strong attractive field, 
and each obstacle emits a repulsive field. By following the sum of all fields at each position, we can construct a path towards
the goal pose.
![PF Demo](https://i.imgur.com/3HYFFsI.gif)

### Virtual Fields
A problem with the potential field planner is that it is easy for the planner to get stuck in local
minima traps. Thus the Virtual Field method proposed by Ding Fu-guang in [this paper](https://ieeexplore-ieee-org.proxy.lib.uwaterloo.ca/stamp/stamp.jsp?tp=&arnumber=1626816)
has been implemented to steer the path towards the open free space in the instance where the path is stuck.
![Virtual Field](https://i.imgur.com/MvO76Wq.gif)


## Grid Based Planner
Grid based planners model the free C-Space as a grid. From there a graph search algorithm is used to 
search the graph for a path from the start and end pose.

A grid based planner is planned for a future release.

# Current Issues
-  Updating starting configuration in PRM doesn't clear search visualization
-  Virtual Field pushes path into obstacles in certain scenarios

# Contributing

Contributions are always welcome!

See `contributing.md` for ways to get started.

# Roadmap
- Add Grid Based Local Planner
- Add variable growth factor to RRT planner
- Add new local planners: RRT* / D* / Voronoi Roadmap
- Add dynamic trajectory generation visualization as shown in [this video](https://www.youtube.com/watch?v=Cj6tAQe7UCY)

# Authors

Project Setup / Algorithm Implementations
- [@abdurj](https://www.github.com/abdurj)

Styling / UI / Design
- [@j232shen](https://github.com/j232shen)

  
# Acknowledgements
**PRM**
- Becker, A. (2020, November 23). PRM: Probabilistic Roadmap Method in 3D and with 7-DOF robot arm. [YouTube](https://www.youtube.com/watch?v=tlFVbHENPCI&feature=youtu.be)
- Modern Robotics, Chapter 10.5: Sampling Methods for Motion Planning (Part 1 of 2). (2018, March 16). [YouTube](https://www.youtube.com/watch?v=rKe6HO8LDu0)
  
**RRT**
- Algobotics: Python RRT Path Planning playlist. [Youtube](https://www.youtube.com/watch?v=TzfNzqjJ2VQ&list=PL9RPomGb9IpRlfQEGkWnTt8jIauPovpOH)
- RRT, RRT* & Random Trees. (2018, November 21). [YouTube](https://www.youtube.com/watch?v=Ob3BIJkQJEw)
  
**Potential Field**
- Ding Fu-guang, Jiao Peng, Bian Xin-qian and Wang Hong-jian, "AUV local path planning based on virtual potential field," IEEE International Conference Mechatronics and Automation, 2005, 2005, pp. 1711-1716 Vol. 4, doi: 10.1109/ICMA.2005.1626816. [URL](https://ieeexplore.ieee.org/document/1626816)
- Michael A. Goodrich, Potential Fields Tutorial [URL](https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.115.3259&rep=rep1&type=pdf)
- Safadi, H. (2007, April 18). Local Path Planning Using Virtual Potential Field. [URL](https://www.cs.mcgill.ca/~Ehsafad/robotics/)

# License
This project is licensed under the terms of the MIT license.