# Multi-Agent path planning in python

## Introduction

This repository consists of the implementation of some multi-agent path-planning algorithms in Python. The following algorithms are currently implemented:

- [Centralized Solutions](#centralized-solutions)
   - [Algorithms](Algorithms)
      - [Prioritized Safe-Interval Path Planning (SIPP)](#prioritized-safe-interval-path-planning)
      - [Conflict-Based Search (CBS)](#conflict-based-search)
   - [Post-Processing](#post-processing)
      - [Post-processing of plan using Temporal Plan Graph](#post-processing-with-tpg)
- [Decentralized solutions](#decentralized-solutions)

## Centralized Solutions

In these methods, it is the responsibility of the central planner to provide a plan to the robots.

### Prioritized Safe-Interval Path Planning

SIPP is a local planner, using which, a collision-free plan can be generated, after considering the static and dynamic obstacles in the environment. In the case of multi-agent path planning, the other agents in the environment are considered as dynamic obstacles. 

**Execution**

For SIPP multi-agent prioritized planning, run:

``` 
cd ./centralized/sipp
python3 multi_sipp.py input.yaml output.yaml
```

**Results**

To visualize the generated results

``` 
python3 visualize_sipp.py input.yaml output.yaml 
```

To record video

``` 
python3 visualize_sipp.py input.yaml output.yaml --video 'sipp.avi' --speed 1
```

|            Test 1 (Success)            |            Test 2 (Failure)            |
|:--------------------------------------:|:--------------------------------------:|
| ![Success](./centralized/sipp/results/success.gif) | ![Failure](./centralized/sipp/results/failure.gif)|

### Conflict Based Search

Conclict-Based Search (CBS), is a multi-agent global path planner.

**Execution**

Run:

``` 
cd ./centralized/cbs
python3 cbs.py input.yaml output.yaml
```

**Results**

To visualize the generated results:

``` shell
python3 ../visualize.py input.yaml output.yaml
```

|           Test 1 (Success)           |           Test 2 (Success)           |
|:------------------------------------:|:------------------------------------:|
|![Success](./centralized/cbs/results/test_2.gif) | ![Failure](./centralized/cbs/results/test_1.gif)|

|               8x8 grid              |              32x32 grid             |
|:-----------------------------------:|:-----------------------------------:|
| ![Test 3](./centralized/cbs/results/test_3.gif) | ![Test 4](./centralized/cbs/results/test_4.gif)|

### Post-Processing

#### Post-processing with TPG

The plan, which is computed in discrete time, can be postprocessed to generate a plan-execution schedule, that takes care of the kinematic constraints as well as imperfections in plan execution.

This work is based on: [Multi-Agent Path Finding with Kinematic Constraints](https://www.aaai.org/ocs/index.php/ICAPS/ICAPS16/paper/view/13183/12711)

Once the plan is generated using CBS, please run the following to generate the plan-execution schedule:

``` shell
cd ./centralized/scheduling
python3 minimize.py ../cbs/output.yaml real_schedule.yaml
```

## Decentralized solutions

In this approach, it is the responsibility of each robot to find a feasible path. Each robot sees other robots as dynamic obstacles, and tries to compute a control velocity which would avoid collisions with these dynamic obstacles.

### Velocity obstacles

**Execution**

```shell
cd ./decentralized/velocity_obstacle
python3 velocity_obstacle.py -f velocity_obstacle.avi
```

**Results**

- Test 1: The robot tries to stay at (0, 0), while avoiding collisions with the dynamic obstacles
- Test 2: The robot moves from (5, 0) to (5, 10), while avoiding obstacles

| Test 1|Test 2|
| :------------: | :------------: |
|![Test1](./decentralized/velocity_obstacle/velocity_obstacle_1.gif)|![Test2](./decentralized/velocity_obstacle/velocity_obstacle_2.gif)|

#### References
- [The Hybrid Reciprocal Velocity Obstacle](http://gamma.cs.unc.edu/HRVO/HRVO-T-RO.pdf)
