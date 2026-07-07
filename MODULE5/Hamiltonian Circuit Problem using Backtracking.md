# Hamiltonian Circuit Problem using Backtracking
### Design and Analysis of Algorithms (DAA) – Complete Engineering Study Guide

> **Topic:** Hamiltonian Circuit using Backtracking  
> **Approach:** Backtracking  
> **Language:** Java  
> **Difficulty:** Engineering Semester Exam Level  
> **Document Type:** GitHub Renderable Markdown

---

# Table of Contents

- [1. Introduction](#1-introduction)
- [2. What is a Hamiltonian Circuit?](#2-what-is-a-hamiltonian-circuit)
- [3. Why Do We Need the Hamiltonian Circuit Problem?](#3-why-do-we-need-the-hamiltonian-circuit-problem)
- [4. When is this Algorithm Used?](#4-when-is-this-algorithm-used)
- [5. Where is it Used? (Applications)](#5-where-is-it-used-applications)
- [6. Graph Theory Basics](#6-graph-theory-basics)
- [7. Hamiltonian Path vs Hamiltonian Circuit](#7-hamiltonian-path-vs-hamiltonian-circuit)
- [8. Problem Statement](#8-problem-statement)
- [9. Understanding the Problem Visually](#9-understanding-the-problem-visually)
- [10. Backtracking Intuition](#10-backtracking-intuition)
- [11. Why Backtracking Works](#11-why-backtracking-works)

---

# 1. Introduction

The **Hamiltonian Circuit Problem** is one of the most important problems in **Graph Theory** and **Design and Analysis of Algorithms (DAA)**.

It asks a simple but computationally difficult question:

> **Can we visit every vertex exactly once and return to the starting vertex?**

Unlike shortest-path problems, the Hamiltonian Circuit problem focuses on **visiting every vertex exactly once**, regardless of edge weights.

This problem belongs to one of the most famous classes of computational problems because:

- It is an **NP-Complete Problem**.
- No known polynomial-time algorithm exists for solving it in general.
- It serves as the basis for solving many optimization problems such as the **Traveling Salesman Problem (TSP)**.

Backtracking is one of the standard approaches taught in engineering courses because it intelligently searches possible paths while eliminating impossible choices early.

---

# 2. What is a Hamiltonian Circuit?

A **Hamiltonian Circuit (Cycle)** is a cycle in a graph that:

- Visits every vertex exactly once.
- Returns to the starting vertex.
- Does not repeat any vertex except the starting vertex at the end.

Suppose we have the graph:

```
      A
     / \
    B---C
     \ /
      D
```

One Hamiltonian Circuit is:

```
A → B → D → C → A
```

Observe carefully:

✔ Every vertex appears exactly once.

✔ The journey ends at the starting vertex.

✔ No vertex is repeated.

---

If we instead have

```
A → B → C → D
```

this is **not** a Hamiltonian Circuit because:

- It does not return to A.

---

If we have

```
A → B → C → B → D → A
```

this is also invalid because vertex **B** is visited twice.

---

## Formal Definition

A Hamiltonian Circuit is a closed path that

- visits every vertex exactly once,
- returns to the initial vertex,
- uses graph edges only.

---

# 3. Why Do We Need the Hamiltonian Circuit Problem?

Many real-world problems require visiting every location exactly once.

Examples include:

- delivery planning
- inspection systems
- robotic movement
- manufacturing machines
- computer chip wiring

The Hamiltonian Circuit provides a mathematical model for these situations.

Instead of checking millions of random paths, algorithms systematically explore feasible paths.

---

## Real-World Motivation

Imagine a robot cleaning a warehouse.

```
Room A ---- Room B
 |            |
 |            |
Room D ---- Room C
```

Goal:

- Visit every room exactly once.
- Return to charging station.

Possible route:

```
A → B → C → D → A
```

This is exactly a Hamiltonian Circuit.

---

## Why Engineers Study It

Because it teaches:

- Recursive problem solving
- Constraint satisfaction
- State-space search
- Backtracking
- Graph traversal
- NP-Complete problems

These concepts appear repeatedly in:

- Artificial Intelligence
- Compiler Design
- Network Design
- Operations Research
- Robotics

---

# 4. When is this Algorithm Used?

The Hamiltonian Circuit algorithm is used whenever:

- Every node must be visited exactly once.
- Revisiting nodes is undesirable.
- The graph size is relatively small.
- Exhaustive search is acceptable.
- Constraint satisfaction is required.

---

## Situations Suitable for Backtracking

Suppose we have

```
City A
City B
City C
City D
```

Need to visit each city exactly once.

Possible search:

```
A
|
+---B
|    |
|    +---C
|         |
|         +---D
|
+---C
|
+---D
```

Backtracking systematically explores these possibilities.

---

## When NOT to Use

Avoid backtracking when:

- Graph contains thousands of vertices.
- Approximate solution is acceptable.
- Fast execution is more important than exactness.

In those cases:

- Heuristics
- Genetic Algorithms
- Approximation Algorithms

are often preferred.

---

# 5. Where is it Used? (Applications)

---

## 1. Traveling Salesman Problem (TSP)

The Traveling Salesman Problem asks:

```
Visit every city exactly once
Return home
Minimize total distance
```

Hamiltonian Circuit forms the structural basis of TSP.

```
Home
 / | \
A--B--C
 \ | /
  D
```

---

## 2. Robotics

Robots inspecting factories often must visit every checkpoint exactly once.

Example:

```
Checkpoint1

Checkpoint2

Checkpoint3

Checkpoint4
```

Efficient movement reduces energy consumption.

---

## 3. PCB (Printed Circuit Board) Design

Engineers design electrical paths connecting components.

The routing software often solves graph traversal problems similar to Hamiltonian cycles.

```
CPU ---- RAM

 |         |

GPU ---- SSD
```

---

## 4. DNA Sequencing

Genome assembly involves finding paths through overlap graphs.

Hamiltonian Path formulations were historically used in computational biology.

---

## 5. Delivery Systems

Delivery companies may require:

- Visit every customer.
- Return to warehouse.
- Avoid duplicate visits.

---

## 6. Network Testing

Network engineers may need to:

- Visit every router
- Test connectivity
- Return to starting router

Represented as:

```
Router1

Router2

Router3

Router4
```

---

## 7. AI Search Problems

Hamiltonian search appears in:

- Puzzle solving
- Planning systems
- State-space search
- Constraint satisfaction

---

## 8. Game Development

Game AI often explores every location once while avoiding repeated states.

---

# 6. Graph Theory Basics

Before understanding Hamiltonian Circuits, recall some graph terminology.

---

## Vertex

A node in the graph.

```
(A)
```

---

## Edge

Connection between two vertices.

```
A ------- B
```

---

## Degree

Number of edges connected to a vertex.

Example:

```
      B
      |
A ----C----D
      |
      E
```

Degree(C) = 4

---

## Path

Sequence of connected vertices.

```
A → C → D
```

---

## Cycle

A path that starts and ends at the same vertex.

```
A → B → C → A
```

---

## Connected Graph

Every vertex can be reached from every other vertex.

```
A---B
|   |
D---C
```

---

## Disconnected Graph

```
A---B

C---D
```

Hamiltonian Circuit is impossible here.

---

## Complete Graph

Every pair of vertices has an edge.

Example:

```
     A
   / | \
  B--C--D
   \ | /
```

Complete graphs almost always have Hamiltonian Circuits.

---

# 7. Hamiltonian Path vs Hamiltonian Circuit

| Hamiltonian Path | Hamiltonian Circuit |
|------------------|--------------------|
| Visits every vertex once | Visits every vertex once |
| Does not return | Returns to starting vertex |
| Open path | Closed cycle |
| Easier condition | Additional closing edge required |

---

Example:

Path

```
A → B → C → D
```

Circuit

```
A → B → C → D → A
```

---

# 8. Problem Statement

Given a graph **G(V,E)**,

determine whether there exists a cycle that

- starts at one vertex,
- visits every vertex exactly once,
- returns to the starting vertex.

---

Input

Graph represented using an adjacency matrix.

Example:

```
     0 1 2 3

0    0 1 1 1

1    1 0 1 0

2    1 1 0 1

3    1 0 1 0
```

Output

```
Hamiltonian Circuit Exists
```

or

```
No Hamiltonian Circuit Exists
```

---

# 9. Understanding the Problem Visually

Consider the graph

```
        (0)
       /   \
     (1)---(2)
       \   /
        (3)
```

Need to visit:

```
0
1
2
3
```

Possible route

```
0
↓

1
↓

3
↓

2
↓

0
```

Visited order

```
0 → 1 → 3 → 2 → 0
```

All vertices visited exactly once.

Hence,

✔ Hamiltonian Circuit exists.

---

Now consider

```
A ---- B

C ---- D
```

There are two disconnected components.

Impossible to visit all vertices in one cycle.

Therefore

```
No Hamiltonian Circuit
```

---

Another example

```
A

|

B

|

C

|

D
```

This is only a straight chain.

You cannot return to A without repeating vertices.

Hence,

No Hamiltonian Circuit.

---

# 10. Backtracking Intuition

Instead of checking every possible ordering blindly, backtracking builds the solution one vertex at a time.

The algorithm asks repeatedly:

```
Can I safely place this vertex next?
```

If the answer is **Yes**, it continues.

If the answer is **No**, it immediately removes that choice and tries another.

This avoids exploring many impossible paths.

---

## Basic Idea

Imagine choosing cities to visit.

```
Start

↓

City A

↓

Choose next city

↓

Valid?

↓

Yes → Continue

No → Undo choice
```

---

Example:

```
Path

0
↓

1
↓

2
↓

?
```

Suppose

```
3
```

cannot be added because there is no connecting edge.

Then

```
0
↓

1
↓

2
```

is abandoned.

Algorithm returns to

```
1
```

and tries another option.

This is called **Backtracking**.

---

## Think of It Like a Maze

```
Start

|

+----Correct Path

|

+----Dead End

      ↑

   Go Back
```

Whenever a dead end is reached, the algorithm retreats to the previous decision point and explores another path.

This "try → fail → undo → retry" strategy is the essence of backtracking.

---

# 11. Why Backtracking Works

Backtracking works because a Hamiltonian Circuit must satisfy strict constraints at every step:

1. Each new vertex must be connected to the previous one.
2. A vertex cannot be visited more than once.
3. After all vertices are visited, the last vertex must connect back to the starting vertex.

Instead of generating every permutation first, the algorithm checks these constraints as it builds the path. Invalid partial solutions are discarded immediately.

### High-Level Process

```text
Start at vertex 0
        │
        ▼
Choose an adjacent unvisited vertex
        │
        ▼
Is the choice valid?
   ├── No ──► Try another vertex
   │
   └── Yes
        │
        ▼
Add vertex to current path
        │
        ▼
Have all vertices been included?
   ├── No ──► Repeat
   │
   └── Yes
        │
        ▼
Does last vertex connect to start?
   ├── Yes ──► Hamiltonian Circuit Found
   └── No ──► Backtrack
```

The algorithm therefore explores only feasible partial paths instead of wasting time on obviously invalid ones.

---

**End of Part 1**

**Next Part (Part 2)** will cover:

- Complete Backtracking Algorithm
- Detailed recursive procedure
- `isSafe()` logic
- State-space tree
- ASCII decision tree
- Execution trace
- Step-by-step dry run on a sample graph
- Complete flowchart of the algorithm

---

# 12. Backtracking Algorithm

The Hamiltonian Circuit problem is solved by **constructing the circuit one vertex at a time**.

Instead of generating all possible permutations of vertices, the algorithm:

1. Starts from a fixed starting vertex.
2. Tries to place a valid next vertex.
3. Continues recursively.
4. If no valid choice exists, removes the last vertex (backtracks).
5. Repeats until either:
   - A Hamiltonian Circuit is found, or
   - Every possibility has been explored.

---

## High-Level Algorithm

```
Start from vertex 0
        │
        ▼
Choose an adjacent unvisited vertex
        │
        ▼
Is it valid?
      /    \
    Yes     No
     │        │
     ▼        ▼
 Add to     Try next
 current     vertex
  path
     │
     ▼
All vertices visited?
     │
   ┌─┴────────────┐
   │              │
  No             Yes
   │              │
   ▼              ▼
Recursive    Last vertex connected
 call        back to start?
                  │
             ┌────┴────┐
             │         │
            Yes       No
             │         │
             ▼         ▼
       Solution     Backtrack
```

---

# 13. Representation of the Solution

We maintain an array called **path[]**.

Initially

```
path = [-1, -1, -1, -1]
```

Choose vertex **0** as the starting vertex.

```
path = [0, -1, -1, -1]
```

As the algorithm progresses

```
path = [0,1,-1,-1]
```

then

```
path = [0,1,2,-1]
```

then

```
path = [0,1,2,3]
```

Finally check

```
3 → 0
```

If the edge exists,

```
0 → 1 → 2 → 3 → 0
```

is a Hamiltonian Circuit.

---

# 14. Step-by-Step Backtracking Procedure

The recursive algorithm follows these steps:

### Step 1

Choose the first vertex.

```
Path

0
```

---

### Step 2

Find an adjacent vertex.

Suppose

```
0
|
1
```

Path becomes

```
0 → 1
```

---

### Step 3

Choose another adjacent unvisited vertex.

```
0 → 1 → 2
```

---

### Step 4

Continue until every vertex is used.

```
0 → 1 → 2 → 3
```

---

### Step 5

Check whether

```
3 → 0
```

exists.

If yes,

```
0 → 1 → 2 → 3 → 0
```

Success.

Otherwise,

Backtrack.

---

# 15. Example Graph

Consider

```
        0
      / | \
     1--2  |
      \ | /
        3
```

Adjacency Matrix

|   |0|1|2|3|
|---|--|--|--|--|
|0|0|1|1|1|
|1|1|0|1|0|
|2|1|1|0|1|
|3|1|0|1|0|

---

Possible Hamiltonian Circuit

```
0
↓

1
↓

2
↓

3
↓

0
```

---

# 16. Dry Run of the Algorithm

Initially

```
path

[0,-1,-1,-1]
```

---

## Position = 1

Possible vertices

```
1

2

3
```

Choose

```
1
```

Path

```
[0,1,-1,-1]
```

---

## Position = 2

Visited

```
0

1
```

Candidates

```
2

3
```

Choose

```
2
```

Path

```
[0,1,2,-1]
```

---

## Position = 3

Visited

```
0

1

2
```

Only remaining

```
3
```

Path

```
[0,1,2,3]
```

---

Final Check

```
3 → 0
```

Edge exists.

Therefore

```
0 → 1 → 2 → 3 → 0
```

Hamiltonian Circuit Found.

---

# 17. What Happens When a Dead End Occurs?

Suppose the graph is

```
0

/ \

1  2

|

3
```

Current path

```
0 → 1 → 3
```

Need another vertex.

Remaining

```
2
```

But

```
3 → 2
```

does not exist.

Dead End.

```
0

↓

1

↓

3

×

```

The algorithm removes

```
3
```

Current path becomes

```
0 → 1
```

Now another choice is tried.

This is called **Backtracking**.

---

# 18. Complete Execution Trace

Suppose the graph

```
0

/|\

1 2

\|/

3
```

Execution

```
Start

↓

0

↓

Try 1

↓

Try 2

↓

Try 3

↓

Complete?

↓

Yes

↓

Edge 3→0 exists

↓

SUCCESS
```

---

Now consider another graph.

Execution

```
0

↓

1

↓

2

↓

Dead End

↓

Backtrack

↓

Try another vertex

↓

Dead End

↓

Backtrack

↓

Try another path

↓

Eventually Success
```

---

# 19. State Space Tree

The search process can be visualized as a tree.

```
                    0
          /---------|---------\
         1          2          3
       /   \      /   \      /   \
      2     3    1     3    1     2
     /       \   ...   ...  ...   ...
    3
```

Each level represents one more vertex added to the path.

Some branches terminate early.

```
                    0
                  / | \
                 1  2  3
                /
               2
              /
             X
```

The branch is abandoned because no valid continuation exists.

---

# 20. Decision Tree Showing Backtracking

```
Start

|

0

|

+------1

|      |

|      +------2

|      |       |

|      |       +------3

|      |              |

|      |          Solution

|      |

|      +------Dead End

|             |

|        Backtrack

|

+------2

|

+------3
```

Notice that the algorithm never continues after reaching a dead end.

Instead,

```
Dead End

↓

Undo choice

↓

Return

↓

Try next option
```

---

# 21. Recursive Call Stack

The recursive function calls look like this.

```
hamCycleUtil(1)

↓

hamCycleUtil(2)

↓

hamCycleUtil(3)

↓

hamCycleUtil(4)

↓

Return
```

During backtracking,

```
hamCycleUtil(4)

↓

Return

↓

hamCycleUtil(3)

↓

Try another choice

↓

hamCycleUtil(4)
```

The stack grows while exploring and shrinks while backtracking.

---

# 22. How `isSafe()` Works

Before placing a vertex into the path, two conditions must be checked.

### Condition 1

The vertex must be adjacent to the previously added vertex.

Suppose

```
Current

0 → 1
```

Trying

```
3
```

Need

```
1 ----- 3
```

If no edge exists,

Reject.

---

### Condition 2

The vertex must not already exist in the path.

Current path

```
0 → 1 → 2
```

Trying

```
1
```

Again

Not allowed.

Every vertex can appear only once.

---

Visual summary

```
Candidate Vertex
        │
        ▼
Adjacent to previous?
      │
  ┌───┴────┐
  │        │
 No       Yes
  │        │
Reject  Already visited?
             │
       ┌─────┴─────┐
       │           │
      Yes         No
       │           │
    Reject      Accept
```

---

# 23. Recursive Algorithm Explained

Pseudo workflow

```
Place starting vertex

↓

Find valid next vertex

↓

Place it

↓

Recursive call

↓

Success?

↓

Yes

↓

Return

↓

No

↓

Remove vertex

↓

Try another

↓

Repeat
```

This continues until either:

- every vertex has been included and the last vertex connects back to the first, or
- all possible choices have been exhausted.

---

# 24. Flowchart of the Complete Algorithm

```text
                    +----------------------+
                    |   Start (Vertex 0)   |
                    +----------+-----------+
                               |
                               v
                  +-------------------------+
                  | Initialize path[0] = 0  |
                  +-----------+-------------+
                              |
                              v
                 +---------------------------+
                 | Select next candidate     |
                 +------------+--------------+
                              |
                              v
                 +---------------------------+
                 | Is candidate safe?        |
                 +------+--------------------+
                        |
          +-------------+-------------+
          |                           |
         No                          Yes
          |                           |
          v                           v
   Try another vertex       Add to current path
                                      |
                                      v
                      +-------------------------------+
                      | All vertices included?        |
                      +---------------+---------------+
                                      |
                          +-----------+-----------+
                          |                       |
                         No                      Yes
                          |                       |
                          v                       v
                  Recursive Call      Last connected to start?
                                              |
                                 +------------+------------+
                                 |                         |
                                No                        Yes
                                 |                         |
                                 v                         v
                           Backtrack               Hamiltonian
                           Remove Vertex             Circuit Found
```

---

**End of Part 2**

**Part 3** covers:

- Time & Space Complexity
- Why Brute Force Fails
- Backtracking vs Brute Force
- Worst-Case Analysis
- Complete Edge Cases
- Disconnected Graphs
- Sparse vs Complete Graphs
- Visual comparisons
- Exam notes and frequently asked theory questions

---

# 25. Time Complexity Analysis

The Hamiltonian Circuit problem is computationally expensive because the algorithm may need to explore many possible vertex arrangements.

## Worst-Case Time Complexity

Let **V** be the number of vertices.

At each recursive level, the algorithm may try multiple candidate vertices.

Approximate search tree:

```
Level 0 :  V choices
Level 1 : V-1 choices
Level 2 : V-2 choices
...
Level V-1 : 1 choice
```

Total possibilities:

```
V × (V-1) × (V-2) × ... × 1

= V!
```

Therefore,

```
Worst Case Time Complexity = O(V!)
```

Although backtracking prunes many invalid branches, the worst case remains factorial.

---

## Why is it Factorial?

Consider four vertices.

Possible orderings:

```
0 1 2 3
0 1 3 2
0 2 1 3
0 2 3 1
0 3 1 2
0 3 2 1
```

Already:

```
4! = 24
```

Now consider:

```
8 vertices

8! = 40,320
```

For

```
10 vertices

10! = 3,628,800
```

For

```
15 vertices

15! ≈ 1.3 trillion
```

The search space grows extremely quickly.

---

## Visual Growth of Search Space

```
Vertices      Possible Orders

2             2
3             6
4             24
5             120
6             720
7             5040
8             40320
9             362880
10            3628800
```

Graphically:

```
Nodes

2    **
3    ******
4    ************************
5    ********************************************************
6    ****************************************************************************************
```

Growth is explosive.

---

# 26. Space Complexity

The recursive algorithm stores:

- current path
- recursion stack
- visited information

Maximum recursion depth equals the number of vertices.

Therefore,

```
Space Complexity = O(V)
```

Components:

```
Path Array

[0,1,2,3]
```

Recursion Stack

```
Level 1

↓

Level 2

↓

Level 3

↓

Level 4
```

Total memory grows linearly with the number of vertices.

---

# 27. Why Brute Force Fails

The brute-force approach generates **every possible ordering of vertices** and then checks whether each ordering forms a Hamiltonian Circuit.

Pseudo process:

```
Generate permutation

↓

Check if valid

↓

Generate next permutation

↓

Check

↓

Repeat
```

For small graphs this works.

For larger graphs, it becomes impractical.

Example:

```
20 vertices

20!

≈ 2.43 × 10^18
```

Even checking one million permutations per second would take many years.

---

## Visualization

Brute Force:

```
Permutation 1

Permutation 2

Permutation 3

Permutation 4

...

Permutation 2,432,902,008,176,640,000
```

Almost every permutation is examined.

---

# 28. How Backtracking Improves Over Brute Force

Backtracking does **not** generate all permutations.

Instead, it rejects impossible paths as soon as they violate the constraints.

Example:

```
Current Path

0 → 1 → 2
```

Trying:

```
4
```

Suppose there is no edge:

```
2 ---- 4
```

Instead of completing the remaining vertices,

Backtracking immediately stops exploring this branch.

```
0

↓

1

↓

2

↓

4

×

Stop
```

No unnecessary work is performed.

---

## Visual Comparison

### Brute Force

```
Root
│
├── Path A
├── Path B
├── Path C
├── Path D
├── Path E
├── Path F
├── Path G
├── ...
```

Every branch is explored.

---

### Backtracking

```
Root
│
├── Path A
│      │
│      ├── Dead End
│      │
│      └── Pruned
│
├── Path B
│      │
│      └── Continue
│
└── Path C
       │
       └── Solution
```

Invalid branches disappear early.

---

# 29. Advantages of Backtracking

## 1. Eliminates Impossible Paths Early

```
Choose

↓

Invalid

↓

Backtrack
```

No further exploration.

---

## 2. Finds Exact Solution

Unlike heuristic algorithms,

Backtracking guarantees correctness.

---

## 3. Memory Efficient

Only one current path is stored.

```
Current Path

0

↓

1

↓

2
```

---

## 4. Simple Recursive Design

The algorithm naturally follows recursive decomposition.

---

## 5. Suitable for Constraint Satisfaction

Examples:

- Sudoku
- N-Queens
- Hamiltonian Circuit
- Graph Coloring

---

# 30. Limitations

Despite its advantages, backtracking has important limitations.

---

## Limitation 1: Exponential Search

Even after pruning,

Worst Case:

```
O(V!)
```

---

Visual

```
Vertices

4

↓

24 possibilities

↓

8

↓

40320 possibilities

↓

12

↓

479001600 possibilities
```

---

## Limitation 2: Large Graphs

For hundreds of vertices,

Backtracking becomes impractical.

```
100 vertices

↓

Impossible within reasonable time
```

---

## Limitation 3: Deep Recursion

Large recursion depth increases stack usage.

```
Call

↓

Call

↓

Call

↓

Call
```

---

## Limitation 4: No Polynomial-Time Guarantee

No known algorithm solves the general Hamiltonian Circuit problem in polynomial time.

---

# 31. Worst-Case Scenario

Worst case occurs when:

- Graph is highly connected.
- Many partial paths appear valid.
- Failure occurs only near the end.

Example:

```
0

/|\

1 2 3

|\|/|

4 5 6
```

Almost every branch must be explored.

The search tree becomes enormous.

---

# 32. Edge Cases

Understanding edge cases is important for exams and interviews.

---

## Case 1: Single Vertex

Graph

```
0
```

Depending on the definition:

- If a self-loop exists, a Hamiltonian Circuit may exist.
- Without a self-loop, most implementations return **No Circuit**.

---

## Case 2: Two Vertices

```
0 ----- 1
```

Need:

```
0 → 1 → 0
```

Only possible if edges exist in both directions (or an undirected edge).

---

## Case 3: Disconnected Graph

```
0 ----- 1


2 ----- 3
```

Impossible.

Reason:

Some vertices cannot be reached.

---

Visual

```
Component A

0---1


Component B

2---3
```

No single cycle can include every vertex.

---

## Case 4: Tree Graph

```
      0
     /
    1
   /
  2
 /
3
```

Trees contain no cycles.

Therefore,

```
Hamiltonian Circuit

Impossible
```

---

## Case 5: Complete Graph

```
      0
    / | \
   1--2--3
    \ | /
```

Every vertex connects to every other vertex.

Many Hamiltonian Circuits exist.

---

## Case 6: Sparse Graph

```
0 -----1

      |

      2

      |

      3
```

Very few edges.

Often impossible to complete a circuit.

---

## Case 7: Graph with Isolated Vertex

```
0 ----1

2
```

Vertex

```
2
```

has no edges.

Impossible.

---

# 33. Visual Comparison of Edge Cases

| Graph Type | Diagram | Hamiltonian Circuit |
|------------|---------|---------------------|
| Complete | `0-1-2-3 (fully connected)` | ✅ Very likely |
| Cycle | `0→1→2→3→0` | ✅ Yes |
| Tree | Branch structure | ❌ No |
| Disconnected | Two separate parts | ❌ No |
| Sparse | Very few edges | Usually ❌ |
| Dense | Many edges | Often ✅ |

---

# 34. Valid vs Invalid Hamiltonian Circuits

## Valid Circuit

```
      0
     / \
    1---2
     \ /
      3
```

Route

```
0 → 1 → 3 → 2 → 0
```

Properties:

- Every vertex visited once
- Returns to start
- Uses existing edges

Result:

```
✓ VALID
```

---

## Invalid Circuit (Repeated Vertex)

```
0 → 1 → 2 → 1 → 3 → 0
```

Problem:

```
Vertex 1

Visited Twice
```

Result:

```
✗ INVALID
```

---

## Invalid Circuit (Missing Vertex)

```
0 → 1 → 3 → 0
```

Vertex

```
2
```

was never visited.

Result:

```
✗ INVALID
```

---

## Invalid Circuit (No Return Edge)

```
0 → 1 → 2 → 3
```

Missing:

```
3 → 0
```

Result:

```
✗ NOT A HAMILTONIAN CIRCUIT
```

---

# 35. Common Mistakes Made by Students

1. Confusing a **Hamiltonian Path** with a **Hamiltonian Circuit**.

2. Forgetting to verify that the last vertex connects back to the starting vertex.

3. Allowing a vertex to appear more than once in the path.

4. Ignoring adjacency before adding a vertex.

5. Assuming every connected graph has a Hamiltonian Circuit.

6. Confusing **Euler Circuit** (visits every edge once) with **Hamiltonian Circuit** (visits every vertex once).

---

# 36. Frequently Asked Theory Questions

### Q1. Is every connected graph Hamiltonian?

**Answer:** No. Connectivity alone is not sufficient.

---

### Q2. Does every complete graph contain a Hamiltonian Circuit?

**Answer:** Yes, every complete graph with at least three vertices has a Hamiltonian Circuit.

---

### Q3. Why is the Hamiltonian Circuit problem difficult?

Because it is an **NP-Complete** problem. No polynomial-time algorithm is known for the general case.

---

### Q4. Why is backtracking preferred over brute force?

Because it prunes invalid partial solutions early, reducing unnecessary exploration while still guaranteeing a correct answer if one exists.

---

### Q5. What is the worst-case complexity?

```
Time  : O(V!)
Space : O(V)
```

---

**End of Part 3**

**Part 4** will include:

- Complete Java implementation with detailed comments
- `isSafe()` helper function
- Recursive backtracking function
- `printSolution()`
- Sample input and output
- Complete dry run of the Java code
- Exam-oriented algorithm, pseudocode, viva questions, and revision summary

---

# 37. Pseudocode

Before writing the Java program, it is useful to understand the algorithm in pseudocode.

```text
HamiltonianCycle(Graph)

    Create path[] of size V

    Initialize all entries as -1

    path[0] = 0

    if HamiltonianUtil(path, 1) == true
        print solution
    else
        print "No Hamiltonian Circuit Exists"
```

Recursive helper function:

```text
HamiltonianUtil(path, position)

    if position == V
        if last vertex is connected to first
            return true
        else
            return false

    for every vertex from 1 to V-1

        if vertex is safe

            add vertex to path

            if HamiltonianUtil(next position)
                return true

            remove vertex (Backtrack)

    return false
```

---

# 38. Complete Java Implementation

```java
public class HamiltonianCircuit {

    // Number of vertices in the graph
    private int V;

    // Constructor
    public HamiltonianCircuit(int vertices) {
        this.V = vertices;
    }

    /*
     * Checks whether the given vertex can be placed
     * at the current position in the Hamiltonian path.
     */
    boolean isSafe(int vertex, int graph[][], int path[], int position) {

        // Check whether there is an edge
        // from previous vertex to current vertex
        if (graph[path[position - 1]][vertex] == 0)
            return false;

        // Check whether vertex already exists
        // in the current path
        for (int i = 0; i < position; i++) {
            if (path[i] == vertex)
                return false;
        }

        return true;
    }

    /*
     * Recursive backtracking function.
     */
    boolean hamCycleUtil(int graph[][], int path[], int position) {

        // Base Case:
        // All vertices are included
        if (position == V) {

            // Check whether last vertex
            // connects to first vertex
            return graph[path[position - 1]][path[0]] == 1;
        }

        // Try different vertices
        for (int vertex = 1; vertex < V; vertex++) {

            if (isSafe(vertex, graph, path, position)) {

                // Place the vertex
                path[position] = vertex;

                // Recur for next position
                if (hamCycleUtil(graph, path, position + 1))
                    return true;

                // Backtrack
                path[position] = -1;
            }
        }

        return false;
    }

    /*
     * Prints Hamiltonian Circuit.
     */
    void printSolution(int path[]) {

        System.out.println("Hamiltonian Circuit Found:");

        for (int i = 0; i < V; i++)
            System.out.print(path[i] + " ");

        // Print starting vertex again
        System.out.println(path[0]);
    }

    /*
     * Driver function.
     */
    void hamCycle(int graph[][]) {

        int path[] = new int[V];

        // Initialize path
        for (int i = 0; i < V; i++)
            path[i] = -1;

        // Start from vertex 0
        path[0] = 0;

        if (!hamCycleUtil(graph, path, 1)) {
            System.out.println("No Hamiltonian Circuit Exists");
            return;
        }

        printSolution(path);
    }

    /*
     * Main Function
     */
    public static void main(String[] args) {

        HamiltonianCircuit h = new HamiltonianCircuit(5);

        int graph[][] = {
                {0,1,0,1,0},
                {1,0,1,1,1},
                {0,1,0,0,1},
                {1,1,0,0,1},
                {0,1,1,1,0}
        };

        h.hamCycle(graph);
    }
}
```

---

# 39. Sample Input

Adjacency Matrix:

```text
0 1 0 1 0
1 0 1 1 1
0 1 0 0 1
1 1 0 0 1
0 1 1 1 0
```

Graph representation:

```text
        0
       / \
      1---3
      |\  |
      | \ |
      2---4
```

---

# 40. Sample Output

```text
Hamiltonian Circuit Found:

0 1 2 4 3 0
```

Another valid circuit may also be produced because multiple Hamiltonian Circuits can exist for the same graph.

---

# 41. Dry Run of the Java Program

Initial state:

```text
path = [0, -1, -1, -1, -1]
```

### Step 1

Try vertex `1`.

```text
path = [0, 1, -1, -1, -1]
```

Safe? **Yes**

---

### Step 2

Try vertex `2`.

```text
path = [0, 1, 2, -1, -1]
```

Safe? **Yes**

---

### Step 3

Try vertex `3`.

```text
2 ---- 3

No edge
```

Not safe.

Backtrack.

```text
path = [0, 1, 2, -1, -1]
```

---

### Step 4

Try vertex `4`.

```text
path = [0, 1, 2, 4, -1]
```

Safe? **Yes**

---

### Step 5

Try remaining vertex.

```text
path = [0,1,2,4,3]
```

All vertices included.

Now verify:

```text
3 ---- 0
```

Edge exists.

Success.

---

# 42. Trace of Recursive Calls

```text
hamCycle()

↓

hamCycleUtil(1)

↓

hamCycleUtil(2)

↓

hamCycleUtil(3)

↓

hamCycleUtil(4)

↓

hamCycleUtil(5)

↓

Solution Found
```

When a choice fails:

```text
hamCycleUtil(3)

↓

Dead End

↓

Return

↓

Undo Choice

↓

Try Next Vertex
```

---

# 43. Complete Algorithm Flow

```text
                Start
                  │
                  ▼
        Initialize path[]
                  │
                  ▼
         path[0] = starting vertex
                  │
                  ▼
        Select candidate vertex
                  │
                  ▼
             isSafe() ?
          ┌──────────────┐
          │              │
         No             Yes
          │              │
          ▼              ▼
     Try another      Add to path
                           │
                           ▼
               All vertices used?
                 ┌─────────────┐
                 │             │
                No            Yes
                 │             │
                 ▼             ▼
           Recursive Call   Last vertex
                             connected
                             to first?
                          ┌────────────┐
                          │            │
                         No           Yes
                          │            │
                          ▼            ▼
                    Backtrack     Print Circuit
```

---

# 44. Correctness of the Algorithm

The algorithm is correct because:

1. Every chosen vertex is adjacent to the previous vertex.
2. No vertex is repeated.
3. Every vertex is eventually included before accepting a solution.
4. The last vertex must connect back to the starting vertex.
5. If a choice leads to failure, backtracking guarantees that every remaining possibility is explored.

Therefore:

- If a Hamiltonian Circuit exists, the algorithm will find one.
- If none exists, every valid possibility is examined before reporting failure.

---

# 45. Advantages and Disadvantages

| Advantages | Disadvantages |
|------------|---------------|
| Finds an exact solution | Factorial worst-case time |
| Simple recursive implementation | Slow for large graphs |
| Prunes invalid paths | NP-Complete problem |
| Less work than brute force | Deep recursion possible |
| Easy to understand | Not scalable to very large inputs |

---

# 46. Hamiltonian Circuit vs Euler Circuit

| Hamiltonian Circuit | Euler Circuit |
|---------------------|---------------|
| Visits every **vertex** exactly once | Visits every **edge** exactly once |
| Vertex repetition not allowed | Vertices may repeat |
| Edge repetition allowed only if vertices satisfy constraints | Edge repetition not allowed |
| NP-Complete problem | Polynomial-time solution exists |
| Solved using backtracking (general case) | Solved using graph traversal algorithms |

### Visual Example

Hamiltonian Circuit:

```text
A → B → C → D → A
```

Each **vertex** is visited once.

Euler Circuit:

```text
Traverse every edge exactly once.
```

The focus is on **edges**, not vertices.

---

# 47. Exam Tips

Remember these key points:

- A Hamiltonian Circuit visits **every vertex exactly once**.
- The path must return to the starting vertex.
- The graph is usually represented using an adjacency matrix.
- Backtracking builds the solution incrementally.
- `isSafe()` performs two checks:
  1. Adjacency
  2. Vertex not previously visited
- Worst-case time complexity: **O(V!)**
- Space complexity: **O(V)**
- The Hamiltonian Circuit problem is **NP-Complete**.

---

# 48. Frequently Asked Viva Questions

### Q1. Why do we fix the starting vertex?

To avoid generating equivalent cycles multiple times and reduce redundant computation.

---

### Q2. What is the role of `isSafe()`?

It ensures that:

- the candidate vertex is adjacent to the previous vertex, and
- it has not already been included in the current path.

---

### Q3. Why is backtracking suitable?

Because it explores only feasible partial solutions and abandons invalid ones early.

---

### Q4. Can a disconnected graph contain a Hamiltonian Circuit?

No. Every vertex must belong to the same connected component.

---

### Q5. Can there be more than one Hamiltonian Circuit?

Yes. A graph may contain multiple valid Hamiltonian Circuits. The presented algorithm stops after finding the first one.

---

### Q6. Is every cycle a Hamiltonian Circuit?

No. A cycle is Hamiltonian only if it includes **every vertex exactly once**.

---

### Q7. Why is the problem NP-Complete?

Because no polynomial-time algorithm is known for solving the general problem, and it is one of the classic NP-Complete decision problems.

---

# 49. Revision Summary

## Definition

A Hamiltonian Circuit is a cycle that visits every vertex exactly once and returns to the starting vertex.

## Technique

Backtracking.

## Data Structure

Adjacency Matrix (commonly used in textbooks).

## Key Helper Function

`isSafe()`

Checks:

- adjacency
- duplicate vertex

## Recursive Function

`hamCycleUtil()`

Responsibilities:

- place vertices
- recurse
- backtrack on failure

## Complexity

| Metric | Complexity |
|--------|------------|
| Time | **O(V!)** |
| Space | **O(V)** |

## Common Applications

- Traveling Salesman Problem
- Robotics
- Circuit Design
- Route Planning
- DNA Sequencing
- AI Search
- Network Testing

---

# 50. Key Takeaways

- The Hamiltonian Circuit problem is a fundamental graph problem in algorithm design.
- Backtracking systematically builds a candidate solution and removes incorrect choices as soon as they are detected.
- Although the worst-case running time is factorial, pruning makes backtracking significantly more efficient than naive brute force in many practical instances.
- Understanding the recursive structure, the `isSafe()` checks, and the backtracking mechanism is essential for semester exams, coding interviews, and advanced graph algorithms.

---

**End of Document**


