# Multi-Agent Path Finding (MAPF)

MAPF problem: find collision-free paths for agents

Alternative names: cooperative path finding (CPF), multi-robot path planning, pebble motion, etc.

## Two Main Directions

1. Improve existing MAPF algorithms (alg efficiency + solution quality)
2. Apply MAPF to readworld applications, deal with different constraints

## Classical MAPF

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

All prior work on classical MAPF forbid vertex conflicts (which implies edge
conflicts are also forbidden).

### Agent Behavior at Target

How an agent behaves when reaching its target and before the last agent reach
its target? Two common assumptions:

- Stay at target: an agent waits in its target until all agents have reached
    their targets. This waiting agent will cause vertex conflicts.

- Disappear at target: when an agent reaches its target it immediately
    disappears.

Most prior work on classical MAPF assumed stay-at-target.

### Objective Functions

Two most common functions used for evaluating MAPF solutions

- **Makespan**: number of time steps required for all agents to reach their
    target.
- **Sum of costs (SOC)**: sum of time steps required by each agent to reach its
    target. SOC is also known as flowtime.

Makespan has been used extensively by compilation-based MAPF algorithms, while
SOC has been used by most search-based MAPF algorithms.

### Complexity

- Optimal single agent path finding is tractable, e.g., Dijkstra's algorithm
- Sub-optimal MAPF (with two free unoccuipied nodes) is tractable, e.g.,
    algorithm Push and Rotate
- MAPF, where agents have joint goal nodes (does not matter which agent reaches
    which goal) is tractable, e.g., reduction to min-cost flow problem
- Optimal (makespan, SOC) MAPF is NP-hard

## Beyond Classical MAPF (Variants)

All the above classical MAPF variants share the following assumptions:

1. time is discretized into time steps;
2. every action takes exactly one time step;
3. in every time step, each agent occupies exactly a single vertex.

MAPF variants that relax these assumptions.

### MAPF on Weighted Graphs

- MAPF in $2^k$-neighbor grids
- MAPF in Euclidean space

### Feasibility Rules

No conflicts is just one type of solution requirement. Some other feasibility
rules:

- Robustness rules: A k-robust MAPF plan builds in a sufficient buffer for
    agents to be delayed up to k time steps without resulting in a conflict.

    **k-robustness**: An agent can visit a vertex, if that vertex has not been
    occupied in recent k steps. 1-robustness covers both swapping conflict and
    following conflict.

- Formation rules: for example, restrictions intended for the agents to maintain
    a specified formation or to maintain a communicaiton link with a set of
    neighboring agents.

### From Pathfinding to Motion Planning

In classical MAPF, agents are assumed to occupy exactly one vertex, in a sense
having no volume, no shape, and move at constant speed. Motion planning
algorithms consider these properties (agent's location, orientation, velocity,
kinematic, etc.).

- MAPF with large agents: agents with volumes
- MAPF with kinematic constraints: the move actions depend not only on current
    location, but also on state parameters such as velocity and orientation

### Tasks and Agents

In classical MAPF, each agent has one task -- to get it to its target.

- Anonymous MAPF: the objective is to move the agents to a set of target
    vertices, but it does not matter which agent reaches which target

- Colored MAPF: a generalization of anonymous MAPF in which agents are grouped
    into teams, and every team has a set of targets. The objective is to move
    the agents in each team to their targets.

- Online MAPF: a sequence of MAPF problems are solved on the same graph (also
    been called "Lifelong MAPF").

    1) Warehouse model: a fixed set of agents solve a MAPF problem, but after an
    agent finds a target, it may be tasked to go to a different target.
    (autonomous warehouses)

    2) Intersection model: new agents may appear, and each agent has one task --
    to reach its target. (autonomous vehicles entering and exiting intersections)

## Benchmarks

- [Moving AI Lab Benchmark](https://movingai.com/benchmarks/mapf.html)
- [Asprilo](https://asprilo.github.io/)
- [MAPF.info](http://mapf.info)

## Algorithms

- A*
- Conflict-Based Search (CBS) [Sharon et al, 2012]

## Reading List

- [  ] Multi-Agent Pathfinding: Definitions, Variants, and Benchmarks, SOCS 2019
- [  ] [Graph Neural Networks for Decentralized Multi-Robot Path Planning, IROS 2020](https://github.com/proroklab/gnn_pathplanning)
- [  ] [Message-Aware Graph Attention Networks for Large-Scale Multi-Robot Path Planning, IEEE RA-L 2021](https://github.com/proroklab/magat_pathplanning)
- [  ] [MAPF-LNS2: Fast Repairing for Multi-Agent Path Finding via Large Neighborhood Search, AAAI 2022](https://github.com/Jiaoyang-Li/MAPF-LNS2)
- [  ] [Anytime Multi-Agent Path Finding via Large Neighborhood Search, IJCAI 2021](https://github.com/Jiaoyang-Li/MAPF-LNS)
- [  ] [Pairwise Symmetry Reasoning for Multi-Agent Path Finding Search. Artifical Intelligence (AIJ)](https://github.com/Jiaoyang-Li/CBSH2-RTC)
- [  ] EECBS: A Bounded-Suboptimal Search for Multi-Agent Path Finding, AAAI 2021
- [  ] Symmetry Breaking for k-Robust Multi-Agent Path Finding, AAAI 2021
- [  ] Lifelong Multi-Agent Path Finding in Large-Scale Warehouses, AAAI 2021
- [  ] Scalable and Safe Multi-Agent Motion Planning with Nonlinear Dynamics and Bounded Disturbances, AAAI 2021
- [  ] [Branch-and-Cut-and-Price for Multi-Agent Pathfinding, IJCAI 2019](https://github.com/ed-lam/bcp-mapf)
- [  ] New Valid Inequalities in Branch-and-Cut-and-Price for Multi-Agent Path Finding, ICAPS 2020
- [  ] [PRIMAL: Pathfinding via Reinforcement and Imitation Multi-Agent Learning](https://github.com/gsartoretti/PRIMAL)
- [  ] [CL-MAPF: Multi-Agent Path Finding for Car-Like robots with kinematic and spatiotemporal constraints, Robotics and Autonomous Systems 2021](https://github.com/APRIL-ZJU/CL-CBS)
- [  ] [Distributed Heuristic Multi-Agent Path Finding with Communication, ICRA 2021](https://github.com/ZiyuanMa/DHC)

## Labs/Researchers

- [mapf.info](http://mapf.info/index.php/Main/Researchers)
- [Sven Koenig, USC](http://idm-lab.org/index.html)
- [Jiaoyang Li, CMU](https://jiaoyangli.me/) (Sven Koenig's student)
- [Qingbiao Li, Cambridge](https://qingbiaoli.github.io/)

## Other Links

- [MAPF with Conflict-Based Search (CBS) and Space-Time A* (STA*)](https://github.com/GavinPHR/Multi-Agent-Path-Finding)
- [Presentation Slides](https://www.bilibili.com/read/cv10556167)
- [Video Presentation](https://www.bilibili.com/video/BV1X54y1h7qm?share_source=copy_web&vd_source=c64806c776b363c1252493349a1f75ad)
