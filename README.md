# Multi-Agent Path Finding (MAPF)

MAPF problem: find collision-free paths for agents

Alternative names: cooperative path finding (CPF), multi-robot path planning, pebble motion, etc.

## Two Main Directions

1. Improve existing MAPF algorithms (alg efficiency + solution quality)
2. Apply MAPF to readworld applications, deal with different constraints

## MAPF Problem Formulation

(there are some issues with the math support)

Input: a tuple `<G,s,t>`, a set of $k$ *agents*

- $G = (V,E)$ is a *graph*
- $s:[1,...,k] -> V$ maps an agent to a source vertex, $t:[1,...,k] -> V$ maps
    an agent to a target vertex, i.e., each agent is assigned to two vertices in the graph (source, target)
- Time is assumed to be discretized
- Action: *move* or *wait*, $a(v)=v'$ means agent at vertex $v$ performs 
    action $a$, then it will be in vertex $v'$ in the next time step, $(v,v')\in E$
- For a sequence of actions $\pi=(a_1, ...,a_n)$ and agent $i$ with source
    $s(i)$, we denote by $\pi_i[x]$ the location of the agent after executing
    the first $x$ actions in $\pi$. 
    Formally, $\pi_i[x] = a_x(a_{x-1}(...a_1(s(i))))$.
- *single-agent plan*: a sequence of actions $\pi$ for a agent leading from its source vertex to its target vertex
- *solution*: a set of $k$ single-agent plans, one for each agent

### Types of Conflicts/Constraints

The goal of MAPF solvers is to find a solution, i.e., a single-agent plan for
each agent, that can be executed without collisions.

Let $\pi_i$ and $\pi_j$ be a pair of single-agent plans.

- Vertex conflict

    Agents are planned to occupy the same vertex at the same time
    step. Formally, there is a vertex conflict between $\pi_i$ and  $\pi_j$ iff
    there exists a time step $x$ that $\pi_i[x] = \pi_j[x]$.

- Edge conflict

    Agents are planned to traverse the same edge at the same time step in the
    same direction. Formally, there is an edge conflict between $\pi_i$ and  $\pi_j$ iff
    there exists a time step $x$ such that $\pi_i[x] = \pi_j[x]$ and  $\pi_i[x+1] = \pi_j[x+1]$.

- Following conflict
    One agent is planned to occupy a vertex that was occupied by another agent
    in the previous time step. Formally, there is a following conflict between $\pi_i$ and  $\pi_j$ iff
    there exists a time step $x$ that $\pi_i[x+1] = \pi_j[x]$.

- Cycle conflict

    A cycle conflict between a set of single-agent plans $\pi_i, \pi_{i+1},
    ...\pi_j$ occurs iff in the same time step every agent moves to a vertex
    that was previously occupied by another agent, forming a "rotating cycle"
    pattern. Formally, a cycle conflict between a set of single-agent plans $\pi_i, \pi_{i+1},
    ...\pi_j$ occurs iff there exists a time step $x$ in which $\pi_i[x+1] = \pi_{i+1}[x]$ and $\pi_{i+1}[x+1] = \pi_{i+2}[x]$ ... and $\pi_j[x+1] = \pi_{i}[x]$.

- Swapping conflict

    The agents are planned to swap locations in a single time step. Formally, there is a swapping conflict between $\pi_i$ and  $\pi_j$ iff
    there exists a time step $x$ such that $\pi_i[x+1] = \pi_j[x]$ and  $\pi_j[x+1] = \pi_i[x]$.

- **k-robustness**: An agent can visit a vertex, if that vertex has not been
    occupied in recent k steps. 1-robustness covers both swapping conflict and
    following conflict.

### Objectives

Some typical criteria:
- **Makespan**: distance between the start time of the first agent and the
    completion time of the last agent, maximum of lengths of plans (end times)
- **Sum of costs (SOC)**: sum of lengths of plans (end times)

### Complexity

- Optimal single agent path finding is tractable, e.g., Dijkstra's algorithm
- Sub-optimal MAPF (with two free unoccuipied nodes) is tractable, e.g.,
    algorithm Push and Rotate
- MAPF, where agents  have joint goal nodes (does not matter which agent reaches
    which goal) is tractable, e.g., reduction to min-cost flow problem
- Optimal (makespan, SOC) MAPF is NP-hard

### Algorithms

- A*
- Conflict-Based Search (CBS) [Sharon et al, 2012]

## Reading List

- Multi-Agent Pathfinding: Definitions, Variants, and Benchmarks, SOCS 2019
- [Graph Neural Networks for Decentralized Multi-Robot Path Planning, IROS 2020](https://github.com/proroklab/gnn_pathplanning)
- [Message-Aware Graph Attention Networks for Large-Scale Multi-Robot Path Planning, IEEE RA-L 2021](https://github.com/proroklab/magat_pathplanning)
- [MAPF-LNS2: Fast Repairing for Multi-Agent Path Finding via Large Neighborhood Search, AAAI 2022](https://github.com/Jiaoyang-Li/MAPF-LNS2)
- [Anytime Multi-Agent Path Finding via Large Neighborhood Search, IJCAI 2021](https://github.com/Jiaoyang-Li/MAPF-LNS)
- [Pairwise Symmetry Reasoning for Multi-Agent Path Finding Search. Artifical Intelligence (AIJ)](https://github.com/Jiaoyang-Li/CBSH2-RTC)
- EECBS: A Bounded-Suboptimal Search for Multi-Agent Path Finding, AAAI 2021
- Symmetry Breaking for k-Robust Multi-Agent Path Finding, AAAI 2021
- Lifelong Multi-Agent Path Finding in Large-Scale Warehouses, AAAI 2021
- Scalable and Safe Multi-Agent Motion Planning with Nonlinear Dynamics and Bounded Disturbances, AAAI 2021
- [Branch-and-Cut-and-Price for Multi-Agent Pathfinding, IJCAI 2019](https://github.com/ed-lam/bcp-mapf)
- New Valid Inequalities in Branch-and-Cut-and-Price for Multi-Agent Path Finding, ICAPS 2020
- [PRIMAL: Pathfinding via Reinforcement and Imitation Multi-Agent Learning](https://github.com/gsartoretti/PRIMAL)
- [CL-MAPF: Multi-Agent Path Finding for Car-Like robots with kinematic and spatiotemporal constraints, Robotics and Autonomous Systems 2021](https://github.com/APRIL-ZJU/CL-CBS)
- [Distributed Heuristic Multi-Agent Path Finding with Communication, ICRA 2021](https://github.com/ZiyuanMa/DHC)

## Labs/Researchers

- [mapf.info](http://mapf.info/index.php/Main/Researchers)
- [Sven Koenig, USC](http://idm-lab.org/index.html)
- [Jiaoyang Li, CMU](https://jiaoyangli.me/) (Sven Koenig's student)
- [Qingbiao Li, Cambridge](https://qingbiaoli.github.io/)

## Other Links

- [MAPF with Conflict-Based Search (CBS) and Space-Time A* (STA*)](https://github.com/GavinPHR/Multi-Agent-Path-Finding)
- [Presentation Slides](https://www.bilibili.com/read/cv10556167)
- [Video Presentation](https://www.bilibili.com/video/BV1X54y1h7qm?share_source=copy_web&vd_source=c64806c776b363c1252493349a1f75ad)
