# Branch and Bound - Knapsack Problem (Design and Analysis of Algorithms)

> **Complete Semester Exam Study Guide**
>
> This document explains the **Branch and Bound algorithm for the 0/1 Knapsack Problem** in detail with theory, diagrams, examples, Java implementation, complexity analysis, and exam-oriented notes.

---

# Table of Contents

- [1. Introduction](#1-introduction)
- [2. What is the Knapsack Problem?](#2-what-is-the-knapsack-problem)
- [3. Types of Knapsack Problems](#3-types-of-knapsack-problems)
- [4. Why Branch and Bound?](#4-why-branch-and-bound)
- [5. When to Use Branch and Bound](#5-when-to-use-branch-and-bound)
- [6. Comparison with Other Methods](#6-comparison-with-other-methods)
- [7. Real World Applications](#7-real-world-applications)
- [8. Basic Terminology](#8-basic-terminology)
- [9. Core Idea of Branch and Bound](#9-core-idea-of-branch-and-bound)
- [10. Components of Branch and Bound](#10-components-of-branch-and-bound)
- [11. Bounding Function](#11-bounding-function)
- [12. Fractional Knapsack Bound](#12-fractional-knapsack-bound)
- [13. Algorithm Overview](#13-algorithm-overview)
- [14. Example Problem Setup](#14-example-problem-setup)
- [15. Summary of Part 1](#15-summary-of-part-1)

---

# 1. Introduction

The **Knapsack Problem** is one of the most famous optimization problems in computer science.

The objective is simple:

> **Select a subset of items that maximizes total profit without exceeding the weight capacity of the knapsack.**

Although the statement appears simple, finding the optimal solution becomes computationally difficult when the number of items increases.

For **0/1 Knapsack**, every item can either be:

- Taken completely (1)
- Not taken (0)

Items **cannot** be divided.

Because every item has two choices, the total number of possible solutions is

\[
2^n
\]

where **n** is the number of items.

For large values of **n**, checking every possible combination becomes impossible within reasonable time.

This is where **Branch and Bound** becomes useful.

Instead of exploring every possible solution, Branch and Bound intelligently eliminates portions of the search space that cannot possibly contain the optimal solution.

---

# 2. What is the Knapsack Problem?

The knapsack problem consists of:

- A set of items
- Each item has:
  - Weight
  - Profit (Value)
- A knapsack having limited capacity

Goal:

> Maximize total profit without exceeding weight capacity.

---

## Mathematical Representation

Given

```
n items

Item i

Weight = wi

Profit = pi
```

Knapsack Capacity

```
W
```

Need to maximize

```
P = Σ pi xi
```

Subject to

```
Σ wi xi ≤ W
```

where

```
xi = 1  if item is selected
xi = 0  otherwise
```

---

## Example

| Item | Weight | Profit |
|-------|--------|---------|
| A | 2 | 40 |
| B | 3 | 50 |
| C | 4 | 65 |
| D | 5 | 35 |

Capacity

```
W = 7
```

Possible selections

| Selected Items | Weight | Profit |
|---------------|---------|---------|
| A | 2 | 40 |
| B | 3 | 50 |
| C | 4 | 65 |
| D | 5 | 35 |
| A+B | 5 | 90 |
| A+C | 6 | 105 |
| B+C | 7 | 115 ✔ |
| C+D | 9 | Invalid |
| A+B+C | 9 | Invalid |

Optimal solution

```
Items B and C

Profit = 115
Weight = 7
```

---

# 3. Types of Knapsack Problems

There are several versions of the knapsack problem.

| Type | Description |
|-------|-------------|
| 0/1 Knapsack | Item either selected or rejected |
| Fractional Knapsack | Items may be divided |
| Multiple Knapsack | Multiple bags |
| Bounded Knapsack | Limited copies of each item |
| Unbounded Knapsack | Unlimited copies available |

---

## This chapter focuses on

# 0/1 Knapsack

Since Branch and Bound is mainly discussed for **0/1 Knapsack** in engineering algorithm courses.

---

# 4. Why Branch and Bound?

Suppose

```
n = 40 items
```

Brute Force explores

```
2^40

≈ 1.09 trillion solutions
```

This is extremely expensive.

---

## Observation

Many partial solutions are already worse than the best solution found.

Exploring them is wasteful.

Example

```
Current Profit = 40

Maximum possible future profit = 20

Upper Bound = 60

Current Best Solution = 80
```

Since

```
60 < 80
```

No solution in this branch can beat the current best.

Therefore

```
Entire branch is discarded.
```

This process is called

# Pruning

---

## Benefits

Branch and Bound

- avoids unnecessary exploration
- guarantees optimal solution
- explores only promising states
- reduces search space significantly

---

## Simple Visualization

Without pruning

```
                Root
             /   |   \
            *    *    *
           /|\   |\   |\
          * * *  * *  * *
         /...
```

Every node explored.

---

With pruning

```
                Root
               /   \
              /     \
             *       *
            / \      X
           *   *   Pruned
          /
         *
```

Large portions disappear.

---

# 5. When to Use Branch and Bound

Branch and Bound is useful when

- Exact optimal solution is required
- Brute force is too expensive
- Dynamic Programming consumes excessive memory
- Search tree can be pruned effectively

---

## Suitable Problems

- 0/1 Knapsack
- Traveling Salesman Problem
- Assignment Problem
- Job Scheduling
- Integer Programming

---

## When NOT to Use

If only an approximate answer is needed

Greedy algorithms are much faster.

If

```
Capacity is small
```

Dynamic Programming is usually better.

---

# 6. Comparison with Other Methods

| Method | Optimal | Fast | Memory | Suitable |
|---------|---------|------|---------|----------|
| Brute Force | Yes | No | Low | Small inputs |
| Greedy | No | Yes | Low | Fractional Knapsack |
| Dynamic Programming | Yes | Medium | High | Small Capacity |
| Branch and Bound | Yes | Usually Faster | Medium | Large Search Space |

---

## Comparison Diagram

```
                     Knapsack

                         │

         ┌───────────────┴───────────────┐

         │                               │

 Fractional                      0/1 Knapsack

         │                               │

     Greedy                   DP / Branch & Bound
```

---

# 7. Real World Applications

Branch and Bound is widely used in optimization.

---

## Cargo Loading

A truck has limited capacity.

Packages have

- weight
- profit

Need maximum revenue.

```
Truck Capacity

┌─────────────────────┐
│                     │
│     Cargo Space     │
│                     │
└─────────────────────┘

Choose best packages.
```

---

## Investment Selection

Each investment

- cost
- expected return

Limited budget.

Need maximum return.

---

## Memory Allocation

Operating systems decide

- which processes remain
- which memory blocks to allocate

while maximizing utilization.

---

## Project Selection

Company has

- limited budget
- multiple projects

Need maximum profit.

---

## Resource Allocation

Examples

- cloud computing
- CPU scheduling
- bandwidth allocation

---

## Military Logistics

Choose supplies under

- aircraft weight limits
- transport constraints

while maximizing effectiveness.

---

## Manufacturing

Machine capacity is limited.

Need maximum production value.

---

# 8. Basic Terminology

Before studying the algorithm, understand these important terms.

---

## Root Node

Starting state.

Nothing has been selected.

```
Level = -1

Profit = 0

Weight = 0
```

---

## Level

Represents

```
Which item is currently being decided.
```

Example

```
Level 0

Decision about Item 1

Level 1

Decision about Item 2

Level 2

Decision about Item 3
```

---

## State

A node in the search tree.

Contains

```
Current Profit

Current Weight

Upper Bound

Level
```

---

## Branch

Decision made.

```
Take item

or

Do not take item
```

---

## Bound

Maximum possible profit obtainable from this node.

If this bound is poor,

the branch is discarded.

---

## Live Node

Node that may still produce a better solution.

---

## Dead Node

Node that cannot improve current answer.

Pruned immediately.

---

## Priority Queue

Most implementations use

```
Max Heap
```

The node having highest bound is explored first.

---

# 9. Core Idea of Branch and Bound

The algorithm repeatedly performs three operations.

```
1. Branch

↓

2. Calculate Bound

↓

3. Prune
```

---

## Step 1

Branch

Create two children.

```
Take current item

Not Take current item
```

---

## Step 2

Compute Upper Bound

Estimate the highest possible profit from that node.

Usually computed using

```
Fractional Knapsack
```

---

## Step 3

Prune

If

```
Bound ≤ Current Best Profit
```

Ignore that node forever.

---

## Visual Flow

```text
               Root
                 │
          ┌──────┴──────┐
          │             │
      Take Item     Don't Take
          │             │
     Compute Bound Compute Bound
          │             │
      Good Bound?    Good Bound?
         /   \          /   \
       Yes   No      Yes    No
       │      X       │      X
   Expand     Prune Expand  Prune
```

---

# 10. Components of Branch and Bound

Every node stores important information.

| Field | Meaning |
|--------|----------|
| Level | Current item index |
| Weight | Current total weight |
| Profit | Current total profit |
| Bound | Maximum possible future profit |

---

Example node

```
Level = 2

Weight = 9

Profit = 70

Bound = 115
```

Meaning

```
We already earned 70.

Maximum achievable profit from this node is 115.
```

If best solution already equals

```
120
```

then

```
115 < 120

Prune.
```

---

# 11. Bounding Function

The **bounding function** estimates the **maximum possible profit** that can still be achieved from the current node.

It does **not** guarantee that this profit is attainable.

Instead, it provides an **optimistic upper bound**.

This helps answer the question:

> **Is it worth exploring this branch further?**

If the bound is not better than the best solution already found, the branch is discarded.

---

## Why an Upper Bound?

Suppose the current node has:

- Current Profit = 60
- Remaining Capacity = 5

If even under the **best possible circumstances** the maximum achievable profit is only **85**, and the current best solution already has profit **90**, then:

```text
Maximum Possible = 85

Current Best = 90

85 < 90

Discard this branch.
```

This avoids exploring many unnecessary nodes.

---

# 12. Fractional Knapsack Bound

To calculate the bound efficiently, Branch and Bound uses the idea of the **Fractional Knapsack**.

Steps:

1. Start with the current node's profit and weight.
2. Add remaining items in decreasing order of **profit/weight ratio**.
3. If an item fits completely, include it.
4. If it does not fit completely, include only the fraction that fills the remaining capacity.
5. The resulting profit is the **upper bound**.

Because fractional items are allowed only for the **bound calculation**, this bound is always optimistic and safe for pruning.

---

## Example

Capacity = **10**

| Item | Weight | Profit | Profit/Weight |
|------|--------|--------|---------------|
| A | 2 | 40 | 20 |
| B | 3 | 30 | 10 |
| C | 5 | 50 | 10 |

Suppose current node already has:

```text
Weight = 2
Profit = 40
Remaining Capacity = 8
```

Now compute bound:

```text
Take B completely

Weight = 5
Profit = 70
```

Remaining capacity:

```text
5
```

Take C completely:

```text
Weight = 10
Profit = 120
```

Bound:

```text
120
```

If C did not fit completely, only a fraction would be added to estimate the bound.

---

# 13. Algorithm Overview

The high-level workflow is:

```text
Start
   │
   ▼
Create Root Node
   │
   ▼
Compute Root Bound
   │
   ▼
Insert into Priority Queue
   │
   ▼
Remove Node with Highest Bound
   │
   ▼
Branch into:
   ├── Take Item
   └── Don't Take Item
   │
   ▼
Compute Bound for Children
   │
   ▼
If Bound > Best Profit
   │
   ├── Yes → Add to Queue
   └── No  → Prune
   │
   ▼
Repeat until Queue is Empty
   │
   ▼
Return Best Profit
```

The search strategy is **Best-First Search**, where the node with the highest bound is expanded first.

---

# 14. Example Problem Setup

The detailed execution and state-space tree will be covered in **Part 2**.

Consider the following instance:

| Item | Weight | Profit | Profit/Weight |
|------|--------|--------|---------------|
| 1 | 2 | 40 | 20.0 |
| 2 | 3 | 50 | 16.67 |
| 3 | 5 | 100 | 20.0 |
| 4 | 4 | 60 | 15.0 |

Knapsack Capacity:

```text
W = 8
```

First, sort by decreasing **profit/weight ratio** (if needed):

| Order | Item | Weight | Profit | Ratio |
|------|------|--------|--------|-------|
| 1 | Item 1 | 2 | 40 | 20.0 |
| 2 | Item 3 | 5 | 100 | 20.0 |
| 3 | Item 2 | 3 | 50 | 16.67 |
| 4 | Item 4 | 4 | 60 | 15.0 |

Initial root node:

| Property | Value |
|----------|------|
| Level | -1 |
| Profit | 0 |
| Weight | 0 |
| Bound | Calculated using Fractional Knapsack |

This node becomes the starting point of the search tree.

In **Part 2**, we will:

- Expand every node step by step.
- Compute bounds for each node.
- Draw the complete ASCII state-space tree.
- Show where pruning occurs.
- Identify the optimal solution visually.

---

# 15. Summary of Part 1

You should now understand:

- What the **0/1 Knapsack Problem** is.
- Why **Branch and Bound** is needed.
- The difference between **Brute Force**, **Greedy**, **Dynamic Programming**, and **Branch and Bound**.
- Key terminology such as **state**, **level**, **bound**, **branch**, **live node**, and **dead node**.
- How the **Fractional Knapsack** technique is used to compute an upper bound.
- The overall workflow of the Branch and Bound algorithm.

# 16. Complete Worked Example (Step-by-Step)

In this section, we solve a complete **0/1 Knapsack Problem** using the **Branch and Bound** algorithm.

This is one of the most important sections for semester exams because it demonstrates:

- Tree construction
- Bound calculation
- Node expansion
- Pruning
- Optimal solution

---

# Problem Statement

Consider the following items:

| Item | Weight | Profit |
|------|--------|--------|
| 1 | 2 | 40 |
| 2 | 3 | 50 |
| 3 | 5 | 100 |
| 4 | 4 | 60 |

Knapsack Capacity

```text
W = 8
```

---

## Step 1: Compute Profit-to-Weight Ratio

Branch and Bound computes the upper bound using the Fractional Knapsack approach.

First calculate:

\[
\frac{Profit}{Weight}
\]

| Item | Weight | Profit | Ratio |
|------|--------|--------|-------|
|1|2|40|20|
|2|3|50|16.67|
|3|5|100|20|
|4|4|60|15|

---

## Sorted Order

Arrange in decreasing ratio.

|Order|Item|Weight|Profit|Ratio|
|------|----|------|------|-----|
|1|Item 1|2|40|20|
|2|Item 3|5|100|20|
|3|Item 2|3|50|16.67|
|4|Item 4|4|60|15|

The search tree follows this order.

---

# Step 2: Initial Root Node

Initially

```text
Level = -1

Weight = 0

Profit = 0
```

Need to calculate its bound.

---

## Bound Calculation

Capacity

```text
8
```

Take Item 1

```text
Weight = 2

Profit = 40
```

Take Item 3

```text
Weight = 7

Profit = 140
```

Remaining capacity

```text
1
```

Next item

Item 2

Weight

```text
3
```

Cannot fully fit.

Take

```text
1/3
```

Profit added

```text
50 × (1/3)

≈16.67
```

Root Bound

```text
140 + 16.67

=156.67
```

---

## Root Node

|Level|Weight|Profit|Bound|
|------|------|------|-----|
|-1|0|0|156.67|

---

ASCII representation

```text
              Root
         P=0  W=0
        Bound=156.67
```

---

# Step 3: Expand Root

Two decisions

```text
Take Item 1

or

Don't Take Item 1
```

---

Tree

```text
                     Root

                  /         \

             Take 1      Skip 1
```

---

# Left Child (Take Item 1)

Weight

```text
2
```

Profit

```text
40
```

Remaining capacity

```text
6
```

Compute bound.

Take Item 3

```text
Weight = 7

Profit =140
```

Remaining

```text
1
```

Take

```text
1/3
```

of Item2

```text
16.67
```

Bound

```text
156.67
```

Node

|Level|Weight|Profit|Bound|
|------|------|------|-----|
|0|2|40|156.67|

---

# Right Child (Skip Item 1)

Weight

```text
0
```

Profit

```text
0
```

Capacity

```text
8
```

Take Item3

```text
Weight=5

Profit=100
```

Remaining

```text
3
```

Take Item2

```text
Weight=8

Profit=150
```

Bound

```text
150
```

Node

|Level|Weight|Profit|Bound|
|------|------|------|-----|
|0|0|0|150|

---

Tree after expansion

```text
                         Root

             --------------------------

             |                        |

      Take Item1              Skip Item1

     P=40 W=2 B=156.67      P=0 W=0 B=150
```

---

# Step 4: Choose Highest Bound

Priority Queue always removes

```text
Highest Bound
```

Current nodes

|Node|Bound|
|----|------|
|Take1|156.67|
|Skip1|150|

Explore

```text
Take Item1
```

---

# Step 5: Expand Take Item1

Decision about Item3

```text
Take

Skip
```

---

## Child A (Take Item3)

Weight

```text
2+5=7
```

Profit

```text
40+100

=140
```

Remaining capacity

```text
1
```

Fraction of Item2

```text
16.67
```

Bound

```text
156.67
```

Node

|Weight|Profit|Bound|
|------|------|-----|
|7|140|156.67|

---

## Child B (Skip Item3)

Weight

```text
2
```

Profit

```text
40
```

Capacity left

```text
6
```

Take Item2

```text
Weight=5

Profit=90
```

Remaining

```text
3
```

Take

```text
3/4
```

of Item4

```text
45
```

Bound

```text
135
```

Node

|Weight|Profit|Bound|
|------|------|-----|
|2|40|135|

---

Tree

```text
                     Root

                 /            \

           Take1             Skip1

          /      \

     Take3      Skip3

 P=140       P=40

 B=156.67    B=135
```

---

# Step 6: Update Best Profit

Current feasible solution

```text
Take Item1

Take Item3
```

Weight

```text
7
```

Profit

```text
140
```

Current Best

```text
140
```

---

# Step 7: Priority Queue Status

Remaining nodes

|Node|Bound|
|----|------|
|Skip1|150|
|Skip3|135|

Current Best

```text
140
```

---

# Pruning Rule

If

```text
Bound ≤ Best Profit
```

Then

```text
Prune
```

Current

```text
Skip3 Bound

135
```

Best

```text
140
```

Since

```text
135 <140
```

Entire subtree removed.

---

Visual

```text
                Skip3

             Bound=135

                 X

             PRUNED
```

---

Tree

```text
                     Root

                 /            \

            Take1            Skip1

            /   \

       Take3    Skip3(X)

               Pruned
```

---

# Step 8: Explore Skip Item1

Node

```text
Profit=0

Bound=150
```

Since

```text
150>140
```

Cannot prune.

Expand.

---

Decision

```text
Take Item3

Skip Item3
```

---

## Take Item3

Weight

```text
5
```

Profit

```text
100
```

Remaining capacity

```text
3
```

Take Item2

```text
Weight=8

Profit=150
```

Bound

```text
150
```

---

Node

|Weight|Profit|Bound|
|------|------|-----|
|5|100|150|

---

## Skip Item3

Weight

```text
0
```

Profit

```text
0
```

Take Item2

```text
Weight=3

Profit=50
```

Remaining

```text
5
```

Take Item4

```text
Weight=7

Profit=110
```

No more items.

Bound

```text
110
```

---

Node

|Weight|Profit|Bound|
|------|------|-----|
|0|0|110|

---

Tree

```text
                       Root

              ----------------------

             |                     |

        Take1                  Skip1

        /   \                 /     \

    Take3  X           Take3      Skip3

                      B=150       B=110
```

---

# Step 9: More Pruning

Current Best

```text
140
```

Node

```text
Bound=110
```

Since

```text
110<140
```

Discard.

```text
Skip3

X
```

---

Tree

```text
                     Root

              -------------------

             |                  |

          Take1             Skip1

         /                  /

     Take3             Take3

       ✔                  ✔

    Profit140         Profit100
```

---

# Step 10: Expand Take Item3 (Right Side)

Current

```text
Weight=5

Profit=100
```

Remaining

```text
3
```

Decision

```text
Take Item2

Skip Item2
```

---

## Take Item2

Weight

```text
8
```

Profit

```text
150
```

Capacity exactly full.

Bound

```text
150
```

---

Update Best

Current

```text
150
```

Previous

```text
140
```

Best

```text
150
```

---

## Skip Item2

Weight

```text
5
```

Profit

```text
100
```

Remaining

```text
3
```

Fraction of Item4

```text
3/4
```

Profit

```text
45
```

Bound

```text
145
```

Since

```text
145<150
```

Prune.

---

Tree

```text
                          Root

                  -------------------

                 |                  |

             Take1             Skip1

             /                 /

        Take3            Take3

                           /   \

                     Take2     Skip2(X)

                    Profit150
```

---

# Final Optimal Solution

Selected Items

```text
Item3

Item2
```

Weight

```text
5+3

=8
```

Profit

```text
100+50

=150
```

---

# Complete State Space Tree

```text
                                    Root
                           P=0 W=0 B=156.67
                              /          \
                             /            \
                Take1                     Skip1
          P=40 W=2 B=156.67          P=0 W=0 B=150
               /      \                  /      \
              /        \                /        \
      Take3         Skip3(X)      Take3      Skip3(X)
 P=140 W=7      B=135<140      P=100 W=5      B=110
 B=156.67                         B=150
                                      /     \
                                     /       \
                             Take2         Skip2(X)
                       P=150 W=8        B=145<150
```

---

# Order of Node Expansion

```text
1 Root

↓

2 Take Item1

↓

3 Take Item3

↓

4 Skip Item1

↓

5 Take Item3

↓

6 Take Item2

↓

Optimal Solution Found
```

---

# Important Observations

- Highest-bound node is always expanded first.
- Bound is computed using the Fractional Knapsack idea.
- Branches whose bound is not better than the current best solution are pruned.
- The search does **not** explore all \(2^n\) subsets.
- Pruning substantially reduces the number of nodes that need to be processed.

---

# Summary of Part 2

In this section, you learned:

- How the search tree is built.
- How upper bounds are computed.
- How the priority queue determines the next node to expand.
- How pruning avoids unnecessary exploration.
- How the optimal solution is obtained without visiting every possible subset.

> **Next (Part 3):** Limitations, edge cases, computational complexity, comparison tables, exam notes, and frequently asked viva questions.


# 17. Limitations of Branch and Bound

Although **Branch and Bound (B&B)** is much faster than brute force for many optimization problems, it is **not always efficient**. Its performance depends heavily on the quality of the **bounding function** and the structure of the input.

---

## Limitation 1: Worst Case Still Exponential

The theoretical worst-case time complexity remains:

```text
O(2^n)
```

This happens when:

- Almost every node has a promising bound.
- Very few branches can be pruned.
- The algorithm explores nearly the entire state-space tree.

### Visualization

Without effective pruning:

```text
                     Root
                  /        \
                ●            ●
              /   \        /   \
            ●      ●     ●      ●
           / \    / \   / \    / \
          ●  ●   ●  ●  ●  ●   ●  ●
         ...
```

Almost every node is visited.

---

## Limitation 2: Depends on Bounding Function

A good upper bound allows aggressive pruning.

A poor upper bound causes many unnecessary node expansions.

### Good Bound

```text
Best Profit = 120

Current Node Bound = 90

90 < 120

✗ Pruned
```

### Poor Bound

```text
Best Profit = 120

Current Node Bound = 200

Must Explore
```

Even if the branch can never actually reach 200, the algorithm cannot safely prune it.

---

## Limitation 3: Priority Queue Overhead

Unlike brute force recursion, Branch and Bound usually maintains a **priority queue (max-heap)**.

Each insertion/removal costs approximately:

```text
O(log N)
```

where **N** is the number of live nodes.

---

## Limitation 4: Large Memory Usage

Many live nodes may exist simultaneously.

Example:

```text
Priority Queue

-----------------------
Node A
Node B
Node C
Node D
Node E
Node F
...
-----------------------
```

The queue can become large for difficult instances.

---

## Limitation 5: Sorting Required

Before computing bounds efficiently, items are generally sorted by:

```text
Profit / Weight Ratio
```

Sorting cost:

```text
O(n log n)
```

Although relatively small compared to the search, it is still part of the preprocessing.

---

# 18. Edge Cases

Understanding edge cases is important for exams and implementation.

---

## Case 1: Empty Knapsack Capacity

Capacity:

```text
W = 0
```

Items:

|Item|Weight|Profit|
|----|------|------|
|1|2|30|
|2|3|40|

Result:

```text
Profit = 0

No item selected
```

Visualization

```text
Capacity

[ ]
```

Nothing fits.

---

## Case 2: No Items

Items:

```text
None
```

Capacity:

```text
10
```

Output

```text
Profit = 0
```

Tree

```text
Root

↓

Finish
```

---

## Case 3: Single Item Fits

|Item|Weight|Profit|
|----|------|------|
|1|5|40|

Capacity

```text
5
```

Result

```text
Take Item

Profit=40
```

Visualization

```text
Knapsack

[ Item1 ]
```

---

## Case 4: Single Item Too Heavy

|Item|Weight|Profit|
|----|------|------|
|1|8|100|

Capacity

```text
5
```

Output

```text
Reject Item

Profit=0
```

---

## Case 5: All Items Too Heavy

|Item|Weight|
|----|------|
|1|10|
|2|12|
|3|15|

Capacity

```text
5
```

Result

```text
No item selected
```

Tree

```text
Root

↓

Reject All

↓

Profit=0
```

---

## Case 6: All Items Fit

Capacity

```text
20
```

Items

|Weight|Profit|
|------|------|
|2|30|
|3|40|
|5|70|

Total weight

```text
10
```

Everything fits.

Result

```text
Take All
```

Visualization

```text
Knapsack

+-------+
|Item1  |
|Item2  |
|Item3  |
+-------+
```

---

## Case 7: Equal Profit-to-Weight Ratios

|Item|Weight|Profit|Ratio|
|----|------|------|-----|
|1|2|20|10|
|2|4|40|10|
|3|5|50|10|

Sorting order is not unique.

Branch and Bound still works correctly because the bound remains valid.

---

## Case 8: Duplicate Items

|Item|Weight|Profit|
|----|------|------|
|1|2|30|
|2|2|30|
|3|2|30|

Each item is treated as a separate object.

---

## Case 9: Very Large Capacity

Capacity exceeds total weight.

```text
Capacity = 100

Total Weight = 25
```

Result

```text
Take every item
```

Minimal branching occurs.

---

# 19. Time Complexity Analysis

The complexity of Branch and Bound depends on how many nodes are explored.

---

## Best Case

Excellent pruning.

Only a few nodes are visited.

Approximate complexity:

```text
O(n log n)
```

Reason:

- Sorting dominates.
- Very few nodes remain in the priority queue.

---

## Average Case

Many branches are pruned.

Typical performance:

```text
Much better than O(2^n)
```

Although no exact formula exists, Branch and Bound often performs efficiently on practical datasets.

---

## Worst Case

Every branch must be explored.

Complexity:

```text
O(2^n)
```

Reason:

Every item creates two choices:

```text
Take

Don't Take
```

Total subsets:

```text
2^n
```

---

## Priority Queue Cost

For each live node:

```text
Insert

Remove
```

Each operation:

```text
O(log N)
```

where **N** is the number of nodes currently stored.

---

# 20. Space Complexity

Memory is primarily used for:

- Priority queue
- Node objects
- Item list

Worst case:

```text
O(2^n)
```

because many live nodes may be stored simultaneously.

Typical practical usage is much lower due to pruning.

---

# 21. Complexity Summary Table

|Case|Time Complexity|Space Complexity|
|----|---------------|----------------|
|Best|O(n log n)|O(n)|
|Average|Better than O(2ⁿ)|Moderate|
|Worst|O(2ⁿ)|O(2ⁿ)|

---

# 22. Comparison with Other Knapsack Algorithms

|Feature|Brute Force|Greedy|Dynamic Programming|Branch and Bound|
|--------|-----------|-------|-------------------|----------------|
|Optimal Solution|✔|✗ (0/1)|✔|✔|
|Pruning|✗|✗|✗|✔|
|Memory Usage|Low|Low|High|Medium|
|Worst Time|O(2ⁿ)|O(n log n)|O(nW)|O(2ⁿ)|
|Suitable for Large Capacity|✔|✔|✗|✔|
|Exact Solution|✔|✗|✔|✔|

---

# 23. Branch and Bound vs Dynamic Programming

## Dynamic Programming

Advantages

- Always polynomial with respect to capacity.
- Simple recurrence relation.
- Predictable running time.

Disadvantages

- Requires a DP table.
- Memory grows with capacity.

Example:

```text
Capacity = 100000

DP Table

Huge
```

---

## Branch and Bound

Advantages

- Does not build a DP table.
- Efficient when pruning is effective.
- Can solve large-capacity instances.

Disadvantages

- Worst case remains exponential.
- Performance depends on the bound.

---

# 24. Advantages of Branch and Bound

- Produces the optimal solution.
- Avoids exploring impossible branches.
- Uses upper bounds to reduce computation.
- Effective for combinatorial optimization.
- Suitable for many NP-hard problems.
- Usually faster than brute force.
- Flexible enough for different optimization problems.

---

# 25. Disadvantages

- Worst-case exponential complexity.
- Performance depends on the quality of the bound.
- Requires additional memory for live nodes.
- More complex to implement than greedy algorithms.
- Priority queue operations introduce overhead.

---

# 26. Exam Tips

Students are commonly asked:

### Q1. Why is Fractional Knapsack used?

Answer:

```text
To compute an optimistic upper bound for pruning.
```

---

### Q2. Does Branch and Bound always give the optimal answer?

```text
Yes.

It never prunes a branch that could contain the optimal solution because pruning is based on a safe upper bound.
```

---

### Q3. Why use a Priority Queue?

```text
To always expand the node with the highest bound first, increasing the chance of finding a good solution early.
```

---

### Q4. Difference between Backtracking and Branch and Bound?

|Backtracking|Branch and Bound|
|-------------|----------------|
|Uses feasibility checking|Uses upper bounds|
|Rejects invalid states|Rejects non-promising states|
|Decision problems|Optimization problems|
|May not use a priority queue|Usually uses a priority queue|

---

### Q5. What is a Live Node?

```text
A node that may still produce a better solution and remains in the priority queue.
```

---

### Q6. What is a Dead Node?

```text
A node that has been pruned or completely explored.
```

---

# 27. Frequently Asked Viva Questions

### 1. What is the objective of the 0/1 Knapsack Problem?

Maximize total profit without exceeding the knapsack capacity.

---

### 2. Why is the problem called "0/1"?

Each item has only two choices:

```text
0 → Do not select

1 → Select
```

---

### 3. Why is the Fractional Knapsack solution not used directly?

Because 0/1 Knapsack does not allow taking fractions of items. Fractions are used **only to estimate the upper bound**.

---

### 4. Is Branch and Bound an exact algorithm?

Yes. It always returns the optimal solution.

---

### 5. Which node is expanded first?

The live node with the **highest upper bound**.

---

### 6. What happens if the bound is less than the current best profit?

The entire subtree rooted at that node is pruned.

---

### 7. Why is sorting based on profit-to-weight ratio performed?

To compute tighter upper bounds using the Fractional Knapsack approach.

---

# 28. Common Mistakes in Exams

❌ Forgetting to sort items before calculating the bound.

❌ Using current profit instead of the upper bound for pruning.

❌ Expanding nodes with smaller bounds before larger ones.

❌ Ignoring the priority queue.

❌ Treating the Fractional Knapsack solution as the final answer.

❌ Pruning a node before comparing its bound with the current best profit.

---

# 29. Key Points to Remember

- Branch and Bound is an **exact optimization algorithm**.
- It explores only **promising nodes**.
- The **Fractional Knapsack** method is used to calculate upper bounds.
- A **priority queue** stores live nodes.
- Nodes with the **highest bound** are expanded first.
- Pruning significantly reduces the search space.
- Worst-case time complexity is still **O(2ⁿ)**.
- In practice, it is often much faster than brute force.

---

# Summary of Part 3

You should now understand:

- The limitations of Branch and Bound.
- Important edge cases and how the algorithm behaves.
- Best, average, and worst-case complexities.
- Comparison with Dynamic Programming and Greedy approaches.
- Advantages, disadvantages, exam tips, and viva questions.

> **Next (Part 4):** Complete commented Java implementation, algorithm pseudocode, dry run, sample input/output, and final revision notes.


# 30. Complete Java Implementation

The following Java program implements the **0/1 Knapsack Problem using Branch and Bound** with a **Priority Queue (Max Heap)**. The upper bound is calculated using the **Fractional Knapsack** method.

```java
import java.util.*;

// Represents an item in the knapsack
class Item {
    int weight;
    int profit;
    double ratio;

    Item(int weight, int profit) {
        this.weight = weight;
        this.profit = profit;
        this.ratio = (double) profit / weight;
    }
}

// Represents a node in the Branch and Bound tree
class Node {
    int level;          // Current level (item index)
    int profit;         // Current profit
    int weight;         // Current weight
    double bound;       // Upper bound

    Node() {
    }
}

public class BranchAndBoundKnapsack {

    // Calculate upper bound using Fractional Knapsack
    static double bound(Node u, int n, int W, Item[] items) {

        // If weight exceeds capacity, return 0
        if (u.weight >= W)
            return 0;

        double profitBound = u.profit;
        int totalWeight = u.weight;

        int j = u.level + 1;

        // Add complete items while possible
        while (j < n && totalWeight + items[j].weight <= W) {
            totalWeight += items[j].weight;
            profitBound += items[j].profit;
            j++;
        }

        // Add fractional part of next item
        if (j < n) {
            profitBound += (W - totalWeight) * items[j].ratio;
        }

        return profitBound;
    }

    public static int knapsack(int W, Item[] items, int n) {

        // Sort by decreasing profit/weight ratio
        Arrays.sort(items, (a, b) -> Double.compare(b.ratio, a.ratio));

        // Max Heap based on bound
        PriorityQueue<Node> pq = new PriorityQueue<>(
                (a, b) -> Double.compare(b.bound, a.bound));

        Node u = new Node();
        Node v = new Node();

        // Root node
        v.level = -1;
        v.profit = 0;
        v.weight = 0;
        v.bound = bound(v, n, W, items);

        pq.add(v);

        int maxProfit = 0;

        while (!pq.isEmpty()) {

            v = pq.poll();

            // Ignore nodes whose bound is not promising
            if (v.bound <= maxProfit)
                continue;

            // Move to next level
            u = new Node();
            u.level = v.level + 1;

            if (u.level >= n)
                continue;

            // -------- Include current item --------

            u.weight = v.weight + items[u.level].weight;
            u.profit = v.profit + items[u.level].profit;

            if (u.weight <= W && u.profit > maxProfit)
                maxProfit = u.profit;

            u.bound = bound(u, n, W, items);

            if (u.bound > maxProfit)
                pq.add(u);

            // -------- Exclude current item --------

            u = new Node();

            u.level = v.level + 1;
            u.weight = v.weight;
            u.profit = v.profit;

            u.bound = bound(u, n, W, items);

            if (u.bound > maxProfit)
                pq.add(u);
        }

        return maxProfit;
    }

    public static void main(String[] args) {

        int capacity = 8;

        Item[] items = {
                new Item(2, 40),
                new Item(3, 50),
                new Item(5, 100),
                new Item(4, 60)
        };

        int n = items.length;

        int answer = knapsack(capacity, items, n);

        System.out.println("Maximum Profit = " + answer);
    }
}
```

---

# 31. Sample Output

```text
Maximum Profit = 150
```

---

# 32. Dry Run of the Program

Input

```text
Capacity = 8

Items

Weight Profit

2      40

3      50

5      100

4      60
```

---

## Step 1

Create Root Node

```text
Level = -1

Weight = 0

Profit = 0
```

Bound

```text
156.67
```

Insert into Priority Queue.

---

## Step 2

Remove Highest Bound

```text
Root
```

Expand

```text
Include Item1

Exclude Item1
```

Queue

```text
Take1

Skip1
```

---

## Step 3

Remove Highest Bound

```text
Take1
```

Expand

```text
Take3

Skip3
```

Best Profit

```text
140
```

---

## Step 4

Skip3

```text
Bound =135
```

Current Best

```text
140
```

Therefore

```text
Pruned
```

---

## Step 5

Expand

```text
Skip1
```

Create

```text
Take3

Skip3
```

---

## Step 6

Expand

```text
Take3
```

Generate

```text
Take2
```

Profit

```text
150
```

Best updated.

---

## Step 7

Remaining nodes

Bound

```text
145

110

135
```

All

```text
<150
```

Pruned.

Algorithm terminates.

---

# 33. Pseudocode

```text
BranchAndBoundKnapsack()

Sort items by Profit/Weight

Create Root Node

Compute Bound

Insert Root into Priority Queue

BestProfit = 0

while Queue is not Empty

    Remove highest bound node

    if Bound <= BestProfit

        continue

    Generate Include Child

    Update Best Profit

    Compute Bound

    Insert if promising

    Generate Exclude Child

    Compute Bound

    Insert if promising

Return BestProfit
```

---

# 34. Algorithm Flowchart (ASCII)

```text
                Start
                   │
                   ▼
          Sort Items by Ratio
                   │
                   ▼
           Create Root Node
                   │
                   ▼
           Compute Root Bound
                   │
                   ▼
          Insert into Max Heap
                   │
                   ▼
          Queue Empty ?
             │        │
            Yes      No
             │        ▼
             │   Remove Best Node
             │        │
             │        ▼
             │  Bound > Best ?
             │      │      │
             │     No     Yes
             │      │       │
             │   Discard    ▼
             │         Generate Children
             │              │
             │              ▼
             │      Compute Bounds
             │              │
             │              ▼
             │     Promising Nodes?
             │          │      │
             │         No     Yes
             │          │       │
             │       Prune   Insert Queue
             │                  │
             └──────────────────┘
                    ▼
               Return Answer
```

---

# 35. Complete State Representation

Every node stores:

|Field|Meaning|
|------|-------|
|Level|Current item index|
|Weight|Total weight so far|
|Profit|Profit earned so far|
|Bound|Maximum possible future profit|

Example

```text
------------------------------

Level = 2

Weight = 7

Profit =140

Bound =156.67

------------------------------
```

---

# 36. Why Fractional Knapsack is Used for Bound?

Suppose

Capacity left

```text
3
```

Next item

```text
Weight =5

Profit =50
```

Instead of stopping,

Branch and Bound assumes

```text
Take 3/5
```

Profit

```text
30
```

Estimated upper bound becomes

```text
Current Profit

+

30
```

Although fractional selection is **not actually allowed**, this optimistic estimate helps determine whether a branch is worth exploring.

---

# 37. Frequently Asked Exam Questions

## Question 1

**Why does Branch and Bound guarantee the optimal solution?**

**Answer**

Because it prunes only those branches whose **upper bound is not greater than the current best profit**. Such branches cannot contain a better solution.

---

## Question 2

**Why use a Priority Queue instead of a Stack?**

**Answer**

The priority queue always expands the node with the highest upper bound first, which usually finds good solutions earlier and increases pruning.

---

## Question 3

**Can Branch and Bound solve Fractional Knapsack?**

**Answer**

It can, but it is unnecessary because the greedy algorithm solves the Fractional Knapsack problem optimally in **O(n log n)**.

---

## Question 4

**What is the difference between a feasible solution and an optimal solution?**

|Feasible Solution|Optimal Solution|
|-----------------|----------------|
|Satisfies the weight constraint|Has the maximum possible profit among all feasible solutions|

---

## Question 5

**What happens if a node exceeds the knapsack capacity?**

It is immediately discarded because it cannot lead to a valid solution.

---

# 38. Revision Sheet (One-Page Summary)

## Definition

Branch and Bound is an **exact optimization technique** that explores a state-space tree while pruning branches that cannot improve the current best solution.

---

## Main Idea

```text
Branch

↓

Calculate Upper Bound

↓

Prune

↓

Repeat
```

---

## Node Fields

```text
Level

Weight

Profit

Bound
```

---

## Data Structure

```text
Priority Queue

(Max Heap)
```

---

## Bound Calculation

Uses

```text
Fractional Knapsack
```

---

## Pruning Condition

```text
If

Bound ≤ Best Profit

↓

Discard Branch
```

---

## Time Complexity

|Case|Complexity|
|----|----------|
|Best|O(n log n)|
|Average|Much better than O(2ⁿ)|
|Worst|O(2ⁿ)|

---

## Space Complexity

```text
Worst Case

O(2ⁿ)
```

---

## Advantages

- Exact solution.
- Effective pruning.
- Often much faster than brute force.
- Applicable to many optimization problems.

---

## Disadvantages

- Worst-case exponential time.
- Requires a good bounding function.
- Priority queue overhead.
- Can consume significant memory.

---

# 39. Important Differences for Exams

|Feature|Backtracking|Branch and Bound|
|--------|------------|----------------|
|Goal|Find feasible solution(s)|Find optimal solution|
|Pruning Basis|Constraint violation|Upper bound|
|Search Order|Usually DFS|Best-first using priority queue|
|Optimization|Not necessarily|Yes|
|Guarantees Optimal Answer|Not always|Yes|

---

# 40. Final Conclusion

The **Branch and Bound algorithm** is one of the most important exact algorithms for solving the **0/1 Knapsack Problem**. Instead of examining every possible subset, it intelligently explores only the most promising branches by computing an optimistic **upper bound** using the **Fractional Knapsack** method.

The algorithm maintains a **priority queue** of live nodes, always expanding the node with the highest bound. Branches that cannot improve the current best solution are **pruned**, significantly reducing the search space in many practical cases.

Although its worst-case time complexity is still **O(2ⁿ)**, Branch and Bound often performs much better than brute force due to effective pruning, making it a valuable technique for solving many combinatorial optimization problems.

---

# 41. Final Exam Revision Checklist

Before your semester exam, ensure that you can:

- Explain the 0/1 Knapsack Problem.
- Describe why Branch and Bound is used.
- Define **live node**, **dead node**, **bound**, and **branch**.
- Compute an upper bound using the Fractional Knapsack method.
- Draw and explain the state-space tree.
- Identify which branches should be pruned.
- Trace the algorithm step by step using a priority queue.
- Write or understand the Java implementation.
- State the best, average, and worst-case complexities.
- Compare Branch and Bound with Greedy, Dynamic Programming, and Backtracking.

---

**End of Document**
