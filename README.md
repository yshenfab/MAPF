# Multi-Agent Path Finding (MAPF)

MAPF problem: find collision-free paths for agents

Alternative names: cooperative path finding (CPF), multi-robot path planning, pebble motion, etc.

## Problem Formulation

- a **graph** (directed or undirected)
- a set of **agents**, each agent is assigned to two locations (nodes) in the graph
    (start, destination)
- each agent can perform either **move** (to a neighboring node) or **wait** actions
- **Plan** is a sequence of actions for the agent leading from its start
    location to its destination

## Different Constraints

- **No-swap constraints**, swap is not allowed, i.e., agents use the same edge at the same time
- **No-train constraints**, agent cannot perform move to approach a node if
    there is another agent at that node
- **k-robustness**: An agent can visit a node, if that node has not been
    occupied in recent k steps. 1-robustness covers both no-swap and no-train
    constraints.
- No plan (path) has a cycle.
- No two plans (paths) visit the same location.
- Waiting is not allowed.
- Some specific locations must be visited.
- ...

## Objectives

Some typical criteria:
- **Makespan**: distance between the start time of the first agent and the
    completion time of the last agent, maximum of lengths of plans (end times)
- **Sum of costs (SOC)**: sum of lengths of plans (end times)

## Complexity

- Optimal single agent path finding is tractable, e.g., Dijkstra's algorithm
- Sub-optimal MAPF (with two free unoccuipied nodes) is tractable, e.g.,
    algorithm Push and Rotate
- MAPF, where agents  have joint goal nodes (does not matter which agent reaches
    which goal) is tractable, e.g., reduction to min-cost flow problem
- Optimal (makespan, SOC) MAPF is NP-hard

## Reading List

1. [Graph Neural Networks for Decentralized Multi-Robot Path Planning, IROS 2020](https://github.com/proroklab/gnn_pathplanning)
2. [Message-Aware Graph Attention Networks for Large-Scale Multi-Robot Path Planning, IEEE RA-L 2021](https://github.com/proroklab/magat_pathplanning)
3. [MAPF-LNS2: Fast Repairing for Multi-Agent Path Finding via Large Neighborhood Search, AAAI 2022](https://github.com/Jiaoyang-Li/MAPF-LNS2)
4. [Anytime Multi-Agent Path Finding via Large Neighborhood Search, IJCAI 2021](https://github.com/Jiaoyang-Li/MAPF-LNuS)
5. [Pairwise Symmetry Reasoning for Multi-Agent Path Finding Search. Artifical Intelligence (AIJ)](https://github.com/Jiaoyang-Li/CBSH2-RTC)

## Labs/Researchers

- https://jiaoyangli.me/
