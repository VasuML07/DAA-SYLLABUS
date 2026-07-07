# Branch and Bound for Traveling Salesman Problem (TSP)

> **Design and Analysis of Algorithms — Complete Exam Study Guide**
>
> **Topic:** Branch and Bound Algorithm for the Traveling Salesman Problem (TSP)
>
> This document is designed for engineering semester examinations and is fully compatible with **GitHub Markdown**.

---

# Table of Contents

- [1. Introduction](#1-introduction)
- [2. What is the Traveling Salesman Problem?](#2-what-is-the-traveling-salesman-problem)
- [3. Why Does the TSP Exist?](#3-why-does-the-tsp-exist)
- [4. Why is TSP NP-Hard?](#4-why-is-tsp-np-hard)
- [5. Why Use Branch and Bound?](#5-why-use-branch-and-bound)
- [6. Real World Applications](#6-real-world-applications)
- [7. Mathematical Formulation](#7-mathematical-formulation)
- [8. Basic Concepts of Branch and Bound](#8-basic-concepts-of-branch-and-bound)
- [9. State Space Tree Representation](#9-state-space-tree-representation)
- [10. Node Representation](#10-node-representation)
- [11. Lower Bound Concept](#11-lower-bound-concept)
- [12. Cost Matrix Reduction](#12-cost-matrix-reduction)
- [13. Branching Strategy](#13-branching-strategy)
- [14. Bounding Strategy](#14-bounding-strategy)
- [15. Search Strategy](#15-search-strategy)

---

# 1. Introduction

The **Traveling Salesman Problem (TSP)** is one of the most famous optimization problems in computer science.

The objective is simple:

> A salesman starts from one city, visits every other city **exactly once**, and finally returns to the starting city while minimizing the total travel cost.

Although the statement is easy to understand, finding the optimal solution becomes extremely difficult as the number of cities increases.

The **Branch and Bound (B&B)** algorithm is one of the most important exact algorithms used to solve the Traveling Salesman Problem more efficiently than brute force by eliminating unnecessary searches.

---

# 2. What is the Traveling Salesman Problem?

Suppose there are **N cities**.

The salesman must:

- Start from one city
- Visit every city exactly once
- Return to the starting city
- Minimize total travel cost

Example:

```
Cities

A
B
C
D
```

Distance Matrix

| From | A | B | C | D |
|------|---|---|---|---|
| A | - |10|15|20|
| B |10|-|35|25|
| C |15|35|-|30|
| D |20|25|30|-|

Possible tour:

```
A → B → D → C → A
```

Cost

```
10 + 25 + 30 + 15 = 80
```

Goal:

```
Find the minimum possible tour.
```

---

# 3. Why Does the TSP Exist?

Many real-world systems require visiting multiple locations while minimizing travel cost.

Examples include:

- Delivery trucks
- Postal services
- Airline scheduling
- School bus routing
- Garbage collection
- Chip manufacturing
- Robot navigation

Without optimization,

- fuel increases
- travel time increases
- operational cost increases

Therefore, optimization algorithms are essential.

---

# Motivation

Imagine a delivery company.

```
Warehouse

↓

Deliver

↓

City 1

↓

City 2

↓

City 3

↓

City 4

↓

Return Warehouse
```

Question:

Which order minimizes total distance?

Trying every possibility quickly becomes impossible.

---

# Number of Possible Tours

For **N cities**

```
(N − 1)!
```

Example

| Cities | Possible Tours |
|---------|---------------|
|3|2|
|4|6|
|5|24|
|6|120|
|7|720|
|8|5040|
|10|362880|
|15|87178291200|

The explosion in possibilities makes brute force impractical.

---

# 4. Why is TSP NP-Hard?

TSP belongs to one of the hardest classes of computational problems.

## Polynomial-Time Algorithms

Problems like

- Sorting
- BFS
- DFS
- Minimum Spanning Tree

can be solved efficiently.

TSP cannot.

---

## Why?

Every possible city order may produce a different total cost.

There is no known algorithm that always finds the optimal solution in polynomial time.

---

### Growth Comparison

| Problem | Complexity |
|----------|------------|
|Binary Search|O(log n)|
|Merge Sort|O(n log n)|
|Dijkstra|O(E log V)|
|MST|O(E log V)|
|TSP Brute Force|O(n!)|

Notice the enormous difference.

---

## Why Brute Force Fails

Suppose

```
10 cities
```

Possible tours

```
9!

=
362880
```

For

```
15 cities

14!

=
87,178,291,200
```

Even modern computers struggle to examine all possibilities.

Hence smarter searching is required.

---

# 5. Why Use Branch and Bound?

Branch and Bound reduces unnecessary work.

Instead of exploring **every** tour,

it intelligently eliminates tours that can never become optimal.

Idea:

```
Generate

↓

Estimate minimum possible cost

↓

If estimate is already worse

↓

Discard immediately
```

This process is called **pruning**.

---

## Comparison

Brute Force

```
Generate ALL tours

↓

Compute cost

↓

Choose minimum
```

Branch and Bound

```
Generate partial tour

↓

Compute lower bound

↓

If promising

↓

Expand

Else

↓

Prune
```

Huge portions of the search tree are skipped.

---

## Advantages

- Exact algorithm
- Finds optimal solution
- Much faster than brute force in practice
- Eliminates impossible solutions early
- Uses mathematical lower bounds

---

## Disadvantages

- Worst case still exponential
- Requires additional memory
- Bounding calculations increase implementation complexity

---

# 6. Real World Applications

## Logistics

```
Warehouse

↓

Customers

↓

Return Warehouse
```

Goal:

Minimum delivery distance.

---

## Courier Services

Companies like

- Amazon
- FedEx
- DHL

must optimize delivery routes.

Benefits

- lower fuel
- faster delivery
- fewer vehicles

---

## Airline Scheduling

Aircraft visiting multiple airports.

Objective:

```
Minimum operating cost
```

---

## PCB Manufacturing

Drilling machine

```
Hole 1

↓

Hole 2

↓

Hole 3

↓

...
```

Movement must be minimized.

---

## Robot Navigation

Autonomous robots

```
Pick object A

↓

Object B

↓

Object C

↓

Return charging station
```

Shortest route saves battery.

---

## DNA Sequencing

Genome assembly often requires solving problems similar to TSP.

---

## Satellite Observation

Satellites visiting observation targets.

Goal

```
Maximum coverage

Minimum fuel
```

---

# 7. Mathematical Formulation

Suppose

```
G = (V,E)
```

where

```
V = Cities

E = Distances
```

Distance

```
d(i,j)
```

Objective

Minimize

```
Σ d(i,j)
```

Subject to

- Every city visited once
- Return to start
- No repeated cities

---

## Example Matrix

```
      A  B  C  D

A     ∞ 10 15 20

B    10 ∞ 35 25

C    15 35 ∞ 30

D    20 25 30 ∞
```

Diagonal entries are

```
∞
```

because visiting the same city is not allowed.

---

# 8. Basic Concepts of Branch and Bound

Branch and Bound has four major components.

```
                Branch & Bound

                     │

        ┌────────────┼────────────┐

        │            │            │

     Branch       Bound       Pruning

                     │

                 Best First
```

---

## 1. Branch

Create child nodes.

Example

```
A

├── B

├── C

└── D
```

Each child represents choosing the next city.

---

## 2. Bound

Estimate the minimum possible cost from this node.

```
Current Cost

+

Estimated Remaining Cost
```

If this estimate is already large,

there is no reason to continue.

---

## 3. Pruning

If

```
Lower Bound

>

Current Best Tour
```

discard that subtree.

```
        Root

      /   |   \

     X    ✓    X
```

Only promising nodes survive.

---

## 4. Search

Most implementations use

```
Priority Queue

↓

Expand node with

smallest lower bound
```

This is called **Least Cost Branch and Bound**.

---

# 9. State Space Tree Representation

For four cities

```
          A

      /    |    \

     B     C     D

   /  \

 C     D

 |

 D
```

Each level represents selecting another city.

Leaf node

```
Complete tour
```

---

## Levels

|Level|Meaning|
|------|-------|
|0|Starting city|
|1|Choose second city|
|2|Choose third city|
|3|Choose fourth city|
|4|Return home|

---

# 10. Node Representation

Every node stores information about a partial solution.

Typical structure

|Field|Purpose|
|------|-------|
|Current City|Current location|
|Visited Cities|Cities already included|
|Current Cost|Travel cost so far|
|Lower Bound|Estimated minimum possible cost|
|Level|Depth in search tree|
|Path|Tour constructed so far|

Example

```
Node

Path

A → B

Current Cost

10

Bound

55

Visited

A,B

Level

1
```

The priority queue always removes the node with the **smallest bound**.

---

# 11. Lower Bound Concept

The lower bound estimates the minimum possible total cost from a partial solution.

General idea

```
Bound

=

Current Cost

+

Minimum Possible Remaining Cost
```

The estimate must **never exceed** the actual minimum possible completion cost.

Otherwise, valid optimal solutions could be pruned incorrectly.

---

## Why Lower Bounds Matter

Suppose

Current best tour

```
80
```

Node X

```
Bound = 92
```

Since

```
92 > 80
```

no completion of Node X can improve the best answer.

Therefore,

```
Prune Node X
```

---

## Characteristics of a Good Lower Bound

A good lower bound should be:

- Easy to compute
- Always less than or equal to the actual remaining cost
- As tight (close to the actual optimum) as possible

A tighter bound leads to more pruning and fewer explored nodes.

---

# 12. Cost Matrix Reduction

One of the most common bounding methods for TSP is **row-column reduction** of the cost matrix.

The idea is:

1. Reduce each row by subtracting its minimum value.
2. Reduce each column by subtracting its minimum value.
3. The sum of all reductions becomes part of the lower bound.

Example:

Original matrix

```
      A   B   C   D

A     ∞  20  30  10
B    15   ∞  16   4
C     3   5   ∞   2
D    19   6  18   ∞
```

After row reduction:

```
Subtract minimum from each row.
```

After column reduction:

```
Subtract minimum from each column.
```

Total reduction cost contributes to the node's lower bound.

This technique significantly improves pruning efficiency.

---

# 13. Branching Strategy

Branching determines how child nodes are generated.

At each node:

```
Current Path

↓

Choose one unvisited city

↓

Create child node
```

Example:

Current path

```
A → B
```

Remaining cities

```
C
D
E
```

Children generated:

```
A → B → C

A → B → D

A → B → E
```

Each child has:

- Updated path
- Updated current cost
- Updated visited set
- New reduced matrix
- New lower bound

The algorithm continues expanding only the most promising child.

---

# 14. Bounding Strategy

Bounding decides whether a node is worth exploring.

General formula:

```
Lower Bound

=

Current Path Cost

+

Estimated Remaining Cost
```

Decision rule:

```
If Bound < Best Cost

Expand

Else

Prune
```

Example:

```
Best Tour = 70
```

Node A

```
Bound = 58

Expand
```

Node B

```
Bound = 81

Prune
```

Node C

```
Bound = 69

Expand
```

Bounding is the key reason Branch and Bound is far more efficient than brute force.

---

# 15. Search Strategy

Most Branch and Bound implementations for TSP use **Least-Cost (Best-First) Search**.

Algorithm flow:

```
Start Node

↓

Compute Lower Bound

↓

Insert into Priority Queue

↓

Remove Node with Smallest Bound

↓

Branch

↓

Compute Bounds for Children

↓

Insert Promising Children

↓

Repeat Until Complete Tour Found
```

Priority Queue example:

|Node|Bound|Status|
|----|------|------|
|A→B|45|Expand First|
|A→C|53|Wait|
|A→D|67|Wait|
|A→E|81|Likely Pruned|

The node with the **smallest lower bound** is always expanded first, increasing the chances of finding an optimal tour early and enabling more aggressive pruning later.

---

# End of Part 1

**Part 2 will cover:**

- Complete Branch and Bound algorithm workflow
- Cost matrix reduction step-by-step
- Detailed 4-city worked example
- Detailed 5-city worked example
- Complete search tree
- ASCII visualizations
- Step-by-step pruning
- Intermediate priority queue states
- Manual dry run suitable for semester exams

---

# 16. Complete Branch and Bound Algorithm Workflow

The Branch and Bound algorithm for TSP systematically explores only the most promising routes.

Instead of generating every possible tour, it repeatedly:

1. Selects the most promising partial path.
2. Computes a lower bound.
3. Expands only if the node can still lead to a better solution.
4. Prunes otherwise.

Overall workflow:

```text
                 Start

                   │

         Create Root Node

                   │

        Compute Lower Bound

                   │

      Insert into Priority Queue

                   │

         Remove Best Node
    (Smallest Lower Bound First)

                   │

        ┌──────────┴──────────┐
        │                     │
     Complete Tour?          No
        │                     │
       Yes                Generate Children
        │                     │
Update Best Cost      Compute Child Bounds
        │                     │
        └──────────┬──────────┘
                   │
      Bound < Current Best ?
           │              │
         Yes             No
           │              │
      Insert Queue     Prune Node

                   │
             Queue Empty?
                   │
                  Yes
                   │
          Optimal Tour Found
```

---

# 17. Step-by-Step Algorithm

## Step 1

Choose the starting city.

Example:

```
Start = A
```

Current path

```
A
```

Visited

```
{A}
```

---

## Step 2

Construct the initial cost matrix.

Example

| |A|B|C|D|
|---|---|---|---|---|
|A|∞|10|15|20|
|B|10|∞|35|25|
|C|15|35|∞|30|
|D|20|25|30|∞|

---

## Step 3

Compute initial lower bound using matrix reduction.

```
Initial Bound

↓

Reduce Rows

↓

Reduce Columns

↓

Add Reduction Cost
```

Suppose

```
Initial Bound = 35
```

Store in root node.

---

## Step 4

Expand the root.

Possible children:

```
A→B

A→C

A→D
```

Each child receives:

- New path
- Updated cost
- New reduced matrix
- New bound

---

## Step 5

Choose node with minimum bound.

Example

|Path|Bound|
|----|------|
|A→B|38|
|A→C|42|
|A→D|47|

Expand

```
A→B
```

---

## Step 6

Repeat until

```
All cities visited

↓

Return to source

↓

Update minimum answer
```

---

# 18. Understanding Cost Matrix Reduction

Branch and Bound usually uses **cost matrix reduction** to compute lower bounds.

Example matrix

```text
      A   B   C   D

A     ∞  20  30  10

B    15   ∞  16   4

C     3   5   ∞   2

D    19   6  18   ∞
```

---

## Row Reduction

Subtract the minimum value from every row.

### Row A

Minimum

```
10
```

Result

```
∞ 10 20 0
```

---

### Row B

Minimum

```
4
```

Result

```
11 ∞ 12 0
```

---

### Row C

Minimum

```
2
```

Result

```
1 3 ∞ 0
```

---

### Row D

Minimum

```
6
```

Result

```
13 0 12 ∞
```

Reduced matrix

```text
      A   B   C   D

A     ∞ 10 20  0

B    11 ∞ 12  0

C     1  3 ∞  0

D    13  0 12 ∞
```

Row reduction cost

```
10 + 4 + 2 + 6

= 22
```

---

## Column Reduction

Column A

```
∞
11
1
13
```

Minimum

```
1
```

Subtract

```
∞
10
0
12
```

Reduction cost

```
1
```

---

Column B

```
10
∞
3
0
```

Minimum

```
0
```

Reduction cost

```
0
```

---

Column C

```
20
12
∞
12
```

Minimum

```
12
```

Subtract

```
8
0
∞
0
```

Reduction cost

```
12
```

---

Column D

Already contains zero.

Reduction cost

```
0
```

---

Final reduced matrix

```text
      A   B   C   D

A     ∞ 10  8  0

B    10 ∞  0  0

C     0  3 ∞  0

D    12  0  0 ∞
```

Total reduction

```
22 + 13

= 35
```

Therefore,

```
Root Lower Bound = 35
```

---

# 19. Four-City Worked Example

Consider

```text
      A   B   C   D

A     ∞ 10 15 20

B    10 ∞ 35 25

C    15 35 ∞ 30

D    20 25 30 ∞
```

Goal

Visit every city exactly once and return to A.

---

## Possible Tours

```
A→B→C→D→A

A→B→D→C→A

A→C→B→D→A

A→C→D→B→A

A→D→B→C→A

A→D→C→B→A
```

Total

```
3!

=

6 tours
```

---

## Tour Costs

### Tour 1

```
A→B→C→D→A

10+35+30+20

=

95
```

---

### Tour 2

```
A→B→D→C→A

10+25+30+15

=

80
```

---

### Tour 3

```
A→C→B→D→A

15+35+25+20

=

95
```

---

### Tour 4

```
A→C→D→B→A

15+30+25+10

=

80
```

---

### Tour 5

```
A→D→B→C→A

20+25+35+15

=

95
```

---

### Tour 6

```
A→D→C→B→A

20+30+35+10

=

95
```

---

Optimal cost

```
80
```

Optimal tours

```
A→B→D→C→A

and

A→C→D→B→A
```

---

# 20. Branch and Bound Search Tree

Instead of checking all six tours, Branch and Bound explores only promising branches.

```text
                    A
          __________|__________
         /          |          \
       AB          AC          AD
      /  \        /  \        /  \
   ABC  ABD    ACB  ACD    ADB  ADC
    |    |      |    |      |    |
  Tour Tour   Tour Tour   Tour Tour
```

Every node stores

```
Path

Cost

Bound
```

Example

```
AB

Cost = 10

Bound = 38
```

---

# 21. Example Node Expansion

Suppose the priority queue contains

|Node|Current Cost|Bound|
|----|------------|-----|
|AB|10|38|
|AC|15|42|
|AD|20|50|

Expand

```
AB
```

Generate

```
ABC

ABD
```

Suppose

|Node|Bound|
|----|-----|
|ABC|74|
|ABD|46|

Priority Queue

|Node|Bound|
|----|------|
|AC|42|
|ABD|46|
|AD|50|
|ABC|74|

Expand

```
AC
```

because

```
42

is minimum
```

---

# 22. How Pruning Works

Suppose the current best solution is

```
80
```

Now a node has

```
Bound

=

91
```

Since

```
91 > 80
```

there is no possibility of improving the answer.

Therefore

```text
          Node

      Bound = 91

           │

       PRUNED ✗
```

Entire subtree disappears.

---

Another example

```text
            Root

        ____|____

      AB   AC   AD

      │     │    │

    Bound Bound Bound

      40    92    45
```

Current Best

```
80
```

Decision

```text
AB  Expand

AC  Prune

AD  Expand
```

Only useful nodes survive.

---

# 23. Priority Queue Evolution

Iteration 1

|Node|Bound|
|----|-----|
|A|35|

---

Iteration 2

|Node|Bound|
|----|-----|
|AB|38|
|AC|42|
|AD|47|

---

Iteration 3

Expand AB

|Node|Bound|
|----|-----|
|AC|42|
|ABD|46|
|AD|47|
|ABC|74|

---

Iteration 4

Expand AC

|Node|Bound|
|----|-----|
|ABD|46|
|AD|47|
|ACD|48|
|ABC|74|

Notice how the smallest-bound node is always selected first.

---

# 24. Comparison with Brute Force

Assume

```
5 Cities
```

Brute Force

```text
Root

├── Tour 1

├── Tour 2

├── Tour 3

├── ...

└── Tour 24
```

Every tour must be evaluated.

---

Branch and Bound

```text
Root

├── Good Branch

│      ├── Continue

│      └── Continue

├── Bad Branch

│      ✗ Pruned

├── Bad Branch

│      ✗ Pruned

└── Good Branch

       Continue
```

Large parts of the tree are never explored.

---

## Comparison Table

|Feature|Brute Force|Branch and Bound|
|--------|-----------|----------------|
|Search|All tours|Promising tours only|
|Pruning|No|Yes|
|Optimal Solution|Yes|Yes|
|Memory|Low|Higher|
|Average Speed|Slow|Much Faster|
|Worst Case|Factorial|Factorial|

---

# 25. Visual Summary

```text
                Root

             Bound = 35

          /      |      \

        38      42      47

       /  \

     74   46

          |

        Complete Tour

        Cost = 80

Current Best = 80

Every node

Bound > 80

↓

PRUNE
```

This illustrates the core idea behind Branch and Bound:

- Use **lower bounds** to estimate future cost.
- Expand only the **most promising** nodes.
- Discard any branch whose bound is already worse than the current best tour.
- Continue until no better solution is possible.

---

# End of Part 2

**Part 3 will cover:**

- Correctness proof
- Time and space complexity
- Symmetric vs. Asymmetric TSP
- Edge cases
- Limitations
- Memory analysis
- Comparison with Greedy, Dynamic Programming, Backtracking, and Brute Force
- Exam notes
- Frequently asked viva and interview questions

---

# 26. Correctness Proof

To prove that the Branch and Bound algorithm always finds the **optimal Traveling Salesman tour**, we establish two important properties.

## Property 1 — Every Feasible Tour Can Be Generated

The algorithm branches by selecting one unvisited city at each level.

For **n** cities:

- Level 0 → Starting city
- Level 1 → Choose second city
- Level 2 → Choose third city
- ...
- Level n−1 → Complete tour

Since every possible city choice is eventually considered (unless pruned), every feasible tour exists somewhere in the search tree.

---

## Property 2 — Pruning Never Removes the Optimal Solution

A node is pruned only when

```text
Lower Bound ≥ Current Best Tour Cost
```

The lower bound is an estimate that is **never greater than** the minimum possible completion cost from that node.

Therefore,

```text
If

Lower Bound ≥ Best Cost

↓

Actual Completion Cost ≥ Best Cost
```

Hence the node cannot produce a better solution.

Therefore pruning is always safe.

---

## Theorem

Since

- every feasible tour is represented,
- pruning removes only nodes that cannot improve the answer,

the algorithm always returns the globally optimal TSP tour.

---

# 27. Time Complexity

Branch and Bound is still an **exact exponential algorithm**.

Worst case:

```text
O(n!)
```

because almost every permutation may need to be explored.

---

## Why Is It Faster in Practice?

The pruning process removes a large number of nodes.

Example

### Brute Force

For 10 cities

```text
9!

=

362,880 tours
```

---

### Branch and Bound

Actual explored nodes may be

```text
15,000

or

5,000

or even

800
```

depending on the effectiveness of the lower bound.

---

## Best Case

Very effective pruning.

Many branches are removed near the root.

Approximate explored nodes

```text
<< n!
```

Although the theoretical complexity remains exponential.

---

## Average Case

Depends upon

- graph structure
- edge weights
- quality of lower bound
- branching order

Usually much faster than brute force.

---

# 28. Space Complexity

Memory is mainly used for

- priority queue
- search tree nodes
- reduced matrices
- visited information

Approximate complexity

```text
O(number of live nodes)
```

Worst case

```text
O(n!)
```

---

## Memory Growth

|Cities|Approximate Memory Need|
|-------|-----------------------|
|4|Very Small|
|8|Small|
|12|Moderate|
|15|Large|
|20|Very Large|

Memory often becomes the practical limitation before computation time.

---

# 29. Symmetric vs Asymmetric TSP

## Symmetric TSP

Distance satisfies

```text
A → B

=

B → A
```

Example

```text
      A  B  C

A     ∞ 10 20

B    10 ∞ 15

C    20 15 ∞
```

Properties

- Undirected graph
- Easier to optimize
- Most textbook examples

---

## Asymmetric TSP

Distance differs.

Example

```text
      A  B  C

A     ∞ 10 20

B    15 ∞ 18

C    11 25 ∞
```

Notice

```text
A→B =10

B→A =15
```

Common in

- one-way roads
- traffic systems
- wind direction
- flight routes

---

## Comparison

|Feature|Symmetric|Asymmetric|
|--------|----------|-----------|
|Distance Matrix|Symmetric|Not Symmetric|
|Graph Type|Undirected|Directed|
|Complexity|Slightly Easier|More Difficult|
|Real Examples|Road Maps|Air Routes|

---

# 30. Limitations of Branch and Bound

Although Branch and Bound is significantly faster than brute force, it has several limitations.

---

## 1. Exponential Worst Case

Worst case

```text
O(n!)
```

Pruning may become ineffective.

Example

```text
Every edge has nearly equal cost.
```

Many nodes appear equally promising.

---

## 2. Large Memory Consumption

Each live node stores

- path
- cost
- bound
- reduced matrix
- visited cities

For many cities,

memory usage grows rapidly.

---

## 3. Lower Bound Quality Matters

Weak lower bound

↓

Poor pruning

↓

Large search tree

Strong lower bound

↓

Excellent pruning

↓

Fast execution

---

## 4. Matrix Reduction Cost

Each node requires

- row reduction
- column reduction

This itself consumes computation time.

---

## 5. Not Suitable for Huge Inputs

Typical limits

```text
20

25

30 cities
```

Beyond this,

heuristic algorithms are usually preferred.

---

# 31. Edge Cases

## Case 1 — Single City

```text
A
```

Tour

```text
A → A
```

Cost

```text
0
```

No branching required.

---

## Case 2 — Two Cities

```text
A

B
```

Only one possible tour

```text
A → B → A
```

Very simple.

---

## Case 3 — Equal Edge Costs

Example

```text
      A B C D

A     ∞ 5 5 5

B     5 ∞ 5 5

C     5 5 ∞ 5

D     5 5 5 ∞
```

Problem

Many nodes obtain

identical lower bounds.

Result

Very little pruning.

---

## Case 4 — Fully Connected Graph

Every city connects to every other city.

```text
A ----- B

|\     /|

| \   / |

|  \ /  |

|  / \  |

| /   \ |

|/     \|

C ----- D
```

Largest branching factor.

---

## Case 5 — Disconnected Graph

```text
A ---- B

C ---- D
```

No Hamiltonian cycle exists.

Algorithm should detect

```text
No feasible tour.
```

---

## Case 6 — Zero Weight Edges

Example

```text
A → B

Cost = 0
```

Matrix reduction still works,

but care must be taken to distinguish

```text
0

and

∞
```

---

# 32. Problematic Scenarios

## Poor Lower Bound

```text
Root

├── 40

├── 41

├── 42

├── 43

├── 44

├── 45

├── 46

├── 47
```

Almost every node looks promising.

Very little pruning.

---

## Strong Lower Bound

```text
Root

├──40

├──90 ✗

├──85 ✗

├──120 ✗

├──42

├──130 ✗

├──115 ✗
```

Only two branches survive.

Huge improvement.

---

# 33. Comparison with Other Algorithms

|Algorithm|Optimal|Pruning|Time Complexity|Remarks|
|-----------|-------|--------|---------------|--------|
|Brute Force|Yes|No|O(n!)|Checks every tour|
|Backtracking|Yes|Partial|O(n!)|Constraint-based pruning|
|Branch and Bound|Yes|Yes|O(n!) Worst Case|Best exact practical approach|
|Dynamic Programming (Held-Karp)|Yes|No|O(n²2ⁿ)|Better theoretical complexity|
|Nearest Neighbor|No|No|O(n²)|Fast heuristic|
|Genetic Algorithm|No|No|Varies|Approximate|
|Ant Colony Optimization|No|No|Varies|Metaheuristic|

---

# 34. Branch and Bound vs Backtracking

|Feature|Backtracking|Branch and Bound|
|--------|------------|----------------|
|Goal|Find feasible solution|Find optimal solution|
|Pruning Based On|Constraint violation|Lower bound|
|Optimization|Indirect|Direct|
|Uses Cost Estimate|No|Yes|
|Priority Queue|Usually No|Yes|
|Guarantees Optimal Answer|Yes (after exploring)|Yes (with bounding)|

---

# 35. Branch and Bound vs Dynamic Programming

|Feature|Branch and Bound|Held-Karp DP|
|--------|----------------|------------|
|Technique|Search Tree|Bitmask DP|
|Worst Case|O(n!)|O(n²2ⁿ)|
|Memory|Variable|O(n2ⁿ)|
|Pruning|Yes|No|
|Implementation|Complex|Moderate|

---

# 36. Advantages

- Produces optimal solution.
- Eliminates many unnecessary branches.
- Faster than brute force in practice.
- Suitable for medium-sized TSP instances.
- General technique applicable to many optimization problems.
- Works well with strong lower bounds.

---

# 37. Disadvantages

- Worst-case exponential complexity.
- High memory usage.
- Performance depends heavily on the bounding function.
- Matrix reduction increases computation.
- Not practical for very large graphs.
- More difficult to implement than brute force.

---

# 38. Exam Tips

Remember the four keywords:

```text
Branch

↓

Bound

↓

Prune

↓

Best-First Search
```

Important formulas

Lower Bound

```text
Current Cost

+

Estimated Remaining Cost
```

Decision

```text
If

Bound < Best Cost

Expand

Else

Prune
```

Worst-case complexity

```text
Time

O(n!)
```

---

# 39. Frequently Asked Viva Questions

### Q1. Why is TSP NP-Hard?

Because no polynomial-time algorithm is known for finding the optimal tour, and the number of possible tours grows factorially.

---

### Q2. What is Branch?

Creating child nodes by selecting the next city.

---

### Q3. What is Bound?

A lower-bound estimate of the minimum possible tour cost from a partial solution.

---

### Q4. Why is pruning correct?

Because the lower bound never exceeds the minimum achievable completion cost, so a pruned node cannot lead to a better solution.

---

### Q5. Why use a priority queue?

To always expand the node with the smallest lower bound first (Least-Cost Branch and Bound).

---

### Q6. Does Branch and Bound always find the optimal solution?

Yes, provided the bound is valid and pruning is performed correctly.

---

### Q7. What is the difference between Branch and Bound and Greedy?

Greedy makes locally optimal choices and may miss the optimal tour. Branch and Bound systematically searches while pruning, guaranteeing the optimal solution.

---

### Q8. Which data structure is commonly used?

A **priority queue (min-heap)** ordered by lower bound.

---

### Q9. What happens if all edge weights are equal?

Lower bounds become similar, pruning is less effective, and the algorithm approaches brute-force behavior.

---

### Q10. What are the main applications of TSP?

- Vehicle routing
- Logistics
- PCB drilling
- Robot path planning
- Airline scheduling
- Genome sequencing

---

# 40. Quick Revision Sheet

## Core Idea

```text
Generate

↓

Estimate

↓

Prune

↓

Repeat
```

## Node Contains

- Current path
- Current city
- Visited cities
- Current cost
- Lower bound
- Level

## Expansion Rule

```text
Expand node with

minimum lower bound
```

## Pruning Rule

```text
Lower Bound ≥ Current Best

↓

Discard Node
```

## Worst-Case Complexity

|Metric|Value|
|------|-----|
|Time|O(n!)|
|Space|O(n!) (worst case)|

## Key Points to Remember

- Branch = create children.
- Bound = estimate minimum possible cost.
- Prune = discard hopeless branches.
- Uses **Best-First Search** with a **priority queue**.
- Always returns the **optimal tour** if implemented correctly.
- Strong lower bounds lead to significant performance gains.

---

# End of Part 3

**Part 4 will include:**

- Complete production-quality Java implementation
- `Node` class
- Priority Queue implementation
- Lower-bound computation
- Full TSP solver
- Worked example (4–5 cities)
- Sample input/output
- Code walkthrough
- Common coding mistakes
- Interview questions
- Final revision summary

  # 41. Complete Java Implementation (Branch and Bound TSP)

> **Note:** This is a compact, exam-oriented implementation using a priority queue and a simple lower-bound function. It demonstrates the complete Branch and Bound workflow while remaining easy to understand.

```java
import java.util.*;

class Node implements Comparable<Node> {
    int city;
    int level;
    int cost;
    int bound;

    List<Integer> path;
    boolean[] visited;

    Node(int n) {
        visited = new boolean[n];
        path = new ArrayList<>();
    }

    @Override
    public int compareTo(Node other) {
        return Integer.compare(this.bound, other.bound);
    }
}

public class BranchBoundTSP {

    static final int INF = 999999;
    static int N;
    static int[][] graph;
    static int bestCost = INF;
    static List<Integer> bestPath = new ArrayList<>();

    // Computes a simple lower bound
    static int calculateBound(Node node) {
        int bound = node.cost;

        for (int i = 0; i < N; i++) {

            if (!node.visited[i]) {

                int min = INF;

                for (int j = 0; j < N; j++) {

                    if (i != j)
                        min = Math.min(min, graph[i][j]);
                }

                bound += min;
            }
        }

        return bound;
    }

    static void solve() {

        PriorityQueue<Node> pq = new PriorityQueue<>();

        Node root = new Node(N);

        root.city = 0;
        root.level = 0;
        root.cost = 0;

        root.path.add(0);
        root.visited[0] = true;

        root.bound = calculateBound(root);

        pq.add(root);

        while (!pq.isEmpty()) {

            Node current = pq.poll();

            if (current.bound >= bestCost)
                continue;

            if (current.level == N - 1) {

                int totalCost = current.cost +
                        graph[current.city][0];

                if (totalCost < bestCost) {

                    bestCost = totalCost;

                    bestPath = new ArrayList<>(current.path);

                    bestPath.add(0);
                }

                continue;
            }

            for (int next = 0; next < N; next++) {

                if (!current.visited[next]) {

                    Node child = new Node(N);

                    child.level = current.level + 1;
                    child.city = next;

                    child.cost = current.cost +
                            graph[current.city][next];

                    child.path = new ArrayList<>(current.path);
                    child.path.add(next);

                    child.visited = current.visited.clone();
                    child.visited[next] = true;

                    child.bound = calculateBound(child);

                    if (child.bound < bestCost)
                        pq.add(child);
                }
            }
        }
    }

    public static void main(String[] args) {

        graph = new int[][]{
                {INF, 10, 15, 20},
                {10, INF, 35, 25},
                {15, 35, INF, 30},
                {20, 25, 30, INF}
        };

        N = graph.length;

        solve();

        System.out.println("Minimum Cost = " + bestCost);

        System.out.print("Optimal Tour = ");

        for (int city : bestPath)
            System.out.print(city + " ");
    }
}
```

---

# Sample Input

```text
      A   B   C   D

A     ∞  10  15  20
B    10   ∞  35  25
C    15  35   ∞  30
D    20  25  30   ∞
```

---

# Expected Output

```text
Minimum Cost = 80

Optimal Tour =

0 1 3 2 0
```

Equivalent city names:

```text
A → B → D → C → A
```

---

# Code Explanation

## Node Class

Stores all information required for one state in the search tree.

```text
Current City

Current Cost

Lower Bound

Visited Cities

Current Path

Tree Level
```

---

## Priority Queue

```java
PriorityQueue<Node> pq = new PriorityQueue<>();
```

The priority queue always removes the node with the **smallest lower bound**, implementing **Least-Cost Branch and Bound**.

---

## Bound Function

```java
calculateBound()
```

Computes:

```text
Current Cost
+
Estimated Minimum Remaining Cost
```

Only nodes with promising bounds are expanded.

---

## Branching

For every unvisited city:

```text
Create Child

↓

Update Cost

↓

Update Path

↓

Update Bound

↓

Insert into Queue
```

---

## Pruning

```java
if(child.bound < bestCost)
    pq.add(child);
```

Nodes whose lower bound is greater than or equal to the current best tour are discarded.

---

# Dry Run

Initial queue:

```text
A

Bound = 35
```

Expand:

```text
A→B

A→C

A→D
```

Priority queue:

```text
AB : 38

AC : 42

AD : 47
```

Expand **AB**.

Generate:

```text
ABC

ABD
```

Eventually:

```text
A→B→D→C→A

Cost = 80
```

Current Best:

```text
80
```

Every node with

```text
Bound ≥ 80
```

is pruned.

---

# Complexity

| Metric | Complexity |
|---------|------------|
| Worst-case Time | **O(n!)** |
| Worst-case Space | **O(n!)** |
| Best Case | Much better due to pruning |
| Search Strategy | Best-First Search |
| Data Structure | Priority Queue (Min Heap) |

---

# Common Coding Mistakes

1. Forgetting to return to the starting city.
2. Using an invalid lower-bound calculation.
3. Not cloning the `visited` array for child nodes.
4. Updating `bestCost` before completing the tour.
5. Expanding nodes without checking the bound.
6. Forgetting to mark cities as visited.
7. Using a max-heap instead of a min-heap.

---

# Frequently Asked Exam Questions

**Q1. Why is a priority queue used?**

To always expand the node with the smallest lower bound first.

**Q2. What is the purpose of the bound?**

It estimates the minimum possible tour cost from the current partial path and enables pruning.

**Q3. Does Branch and Bound always give the optimal solution?**

Yes, because it prunes only branches that cannot improve the current best solution.

**Q4. What is the pruning condition?**

```text
If Lower Bound ≥ Best Cost

↓

Prune the node
```

**Q5. What is the worst-case complexity?**

```text
Time  : O(n!)
Space : O(n!)
```

---

# One-Page Revision Summary

```text
Traveling Salesman Problem

↓

Visit every city exactly once

↓

Return to start

↓

Minimize total cost
```

```text
Branch

↓

Create child nodes
```

```text
Bound

↓

Estimate minimum possible cost
```

```text
Prune

↓

Discard nodes with

Bound ≥ Current Best
```

```text
Priority Queue

↓

Expand node with

Smallest Bound
```

**Key Formula**

```text
Lower Bound
=
Current Cost
+
Estimated Remaining Cost
```

**Key Advantages**

- Exact algorithm
- Optimal solution
- Faster than brute force due to pruning

**Limitations**

- Worst-case exponential time
- High memory usage
- Performance depends on the quality of the lower bound

---

# Conclusion

Branch and Bound is one of the most effective **exact algorithms** for solving the Traveling Salesman Problem. By combining **systematic search**, **lower-bound estimation**, **best-first exploration**, and **pruning**, it dramatically reduces the number of tours that must be examined compared with brute force while still guaranteeing the optimal solution. It remains a fundamental algorithm in the Design and Analysis of Algorithms curriculum and is widely used to illustrate optimization techniques for NP-hard problems.
