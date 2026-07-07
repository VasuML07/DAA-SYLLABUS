# Branch and Bound for the Assignment Problem
### Complete Engineering Study Guide (Design and Analysis of Algorithms)

> Semester Exam Notes | GitHub Markdown | Complete Reference

---

# Table of Contents

- [1. Introduction](#1-introduction)
- [2. Assignment Problem](#2-assignment-problem)
- [3. Why Branch and Bound?](#3-why-branch-and-bound)
- [4. When Should We Use Branch and Bound?](#4-when-should-we-use-branch-and-bound)
- [5. Real-World Applications](#5-real-world-applications)
- [6. Basic Terminologies](#6-basic-terminologies)
- [7. Idea Behind Branch and Bound](#7-idea-behind-branch-and-bound)
- [8. State Space Tree](#8-state-space-tree)
- [9. Bounding Mechanism](#9-bounding-mechanism)
- [10. Branching Strategy](#10-branching-strategy)
- [11. Search Strategy](#11-search-strategy)
- [12. Cost Matrix](#12-cost-matrix)
- [13. Lower Bound Calculation](#13-lower-bound-calculation)

---

# 1. Introduction

The **Assignment Problem** is one of the most important optimization problems in computer science and operations research.

It deals with assigning a set of workers to a set of jobs such that:

- Every worker performs exactly one job.
- Every job is assigned to exactly one worker.
- The **total cost** is minimized (or total profit is maximized).

Suppose there are **n workers** and **n jobs**.

Each worker requires a different cost to perform each job.

Our objective is to find the assignment having the **minimum total cost**.

---

## Example

| Worker | Job A | Job B | Job C |
|---------|-------|-------|-------|
| W1 | 9 | 2 | 7 |
| W2 | 6 | 4 | 3 |
| W3 | 5 | 8 | 1 |

Possible assignment:

```
W1 → Job B
W2 → Job A
W3 → Job C
```

Total Cost

```
2 + 6 + 1 = 9
```

The challenge is finding the **minimum** among all possible assignments.

---

# 2. Assignment Problem

The Assignment Problem belongs to the class of **combinatorial optimization problems**.

It is closely related to:

- Scheduling
- Resource allocation
- Task allocation
- Machine assignment

---

## Mathematical Representation

Let

```
Cost[i][j]
```

represent the cost of assigning

```
Worker i
```

to

```
Job j
```

We need to choose exactly one value from every row and every column such that:

```
Total Cost is Minimum
```

---

## Constraints

Every worker

```
Assigned exactly one job
```

Every job

```
Assigned exactly one worker
```

No duplicate assignments are allowed.

---

## Cost Matrix

Example

```
           Jobs

          J1 J2 J3 J4

W1        9  2  7  8

W2        6  4  3  7

W3        5  8  1  8

W4        7  6  9  4
```

Rows

```
Workers
```

Columns

```
Jobs
```

Each cell

```
Assignment Cost
```

---

# 3. Why Branch and Bound?

## Motivation

The simplest solution is:

```
Generate all assignments

↓

Calculate cost

↓

Choose minimum
```

Unfortunately,

Number of assignments

```
n!
```

Example

| Workers | Assignments |
|----------|-------------|
|3|6|
|4|24|
|5|120|
|6|720|
|7|5040|
|8|40320|
|10|3,628,800|
|15|1.3 trillion|

Clearly,

Brute Force becomes impossible for large values of **n**.

---

## Brute Force Visualization

```
               Root

          / / / | \ \

         All Possible

         Assignments

     / / / / / / / / /

Evaluate Every Solution

          ↓

Choose Minimum
```

Huge amount of unnecessary work.

---

## Branch and Bound Solution

Instead of exploring everything,

it intelligently removes impossible or expensive choices.

```
          Root

        /   |   \

       A    B    C

      / \   X   / \

     D   X     E   X

Only promising nodes are explored.
```

Here

```
X

= Pruned
```

---

## Advantages

✔ Much faster than brute force

✔ Finds optimal solution

✔ Avoids unnecessary computation

✔ Uses lower bound estimation

✔ Suitable for optimization problems

---

## Main Idea

Instead of asking

```
"What is the answer?"
```

it asks

```
"Can this branch ever become the best solution?"
```

If the answer is

```
NO
```

Entire subtree is discarded.

---

# 4. When Should We Use Branch and Bound?

Branch and Bound is useful when

---

## Condition 1

The problem is

```
Optimization Problem
```

Examples

- Minimum cost
- Maximum profit
- Minimum distance

---

## Condition 2

Many possible solutions exist.

Example

```
10!

=

3,628,800
```

Trying all is expensive.

---

## Condition 3

A good

```
Lower Bound

or

Upper Bound
```

can be computed.

Without a good bound,

Branch and Bound becomes inefficient.

---

## Condition 4

Exact solution is required.

Unlike greedy algorithms,

Branch and Bound guarantees

```
Optimal Solution
```

---

## Suitable Problems

✔ Assignment Problem

✔ Traveling Salesman Problem

✔ 15 Puzzle

✔ Job Scheduling

✔ Knapsack (0/1)

✔ Integer Programming

---

# 5. Real-World Applications

---

## 1. Employee Scheduling

Assign employees to shifts.

Goal

```
Minimum salary cost

Maximum productivity
```

---

## 2. Machine Allocation

Assign

```
Machines

↓

Jobs
```

Objective

```
Minimum processing time
```

---

## 3. Delivery Assignment

Assign

```
Drivers

↓

Delivery Routes
```

Minimize

```
Fuel Cost
```

---

## 4. Airline Crew Scheduling

Assign

```
Pilots

↓

Flights
```

while minimizing

- Delay
- Cost
- Overtime

---

## 5. Cloud Computing

Assign

```
Servers

↓

Tasks
```

Goal

```
Minimum execution cost
```

---

## 6. Hospital Management

Assign

```
Doctors

↓

Patients
```

Minimize waiting time.

---

## 7. Manufacturing

Assign

```
Workers

↓

Machines
```

Improve productivity.

---

## 8. Robotics

Assign

```
Robots

↓

Targets
```

to minimize movement cost.

---

# 6. Basic Terminologies

---

## State

One partial assignment.

Example

```
Worker1 → Job2
```

---

## Root Node

Initial state.

```
No assignments made.
```

---

## Child Node

One additional assignment.

Example

```
W1 → J2

↓

W2 → J1
```

---

## Leaf Node

Complete assignment.

Every worker has one job.

---

## Branch

One decision.

Example

```
Assign Worker 2

↓

Job 4
```

---

## Bound

Estimated minimum cost obtainable from current node.

---

## Live Node

Still eligible for expansion.

---

## Dead Node

Cannot produce better answer.

Discarded.

---

## Best Node

Node having

```
Smallest Lower Bound
```

It is expanded next.

---

# 7. Idea Behind Branch and Bound

The algorithm repeatedly performs

```
Choose Best Node

↓

Calculate Lower Bound

↓

Expand Node

↓

Generate Children

↓

Prune Bad Nodes

↓

Repeat
```

until the optimal assignment is obtained.

---

## High-Level Flow

```
               Start

                 |

          Root Node Created

                 |

      Compute Lower Bound

                 |

      +------------------+

      |                  |

Expand Best Node     Ignore Others

      |

Generate Children

      |

Compute Bounds

      |

Prune Expensive Nodes

      |

Repeat

      |

Optimal Assignment
```

---

# 8. State Space Tree

Every node represents

```
Partial Assignment
```

Example

```
               Root

         (No Assignment)

       /      |      \

     J1      J2      J3

    / \      |       / \

  J2 J3     J1     J1 J2

  ...

```

Levels

```
Level 0

No worker assigned

↓

Level 1

Worker 1 assigned

↓

Level 2

Worker 2 assigned

↓

...

↓

Level n

Complete Assignment
```

---

## General Tree Structure

```
Level 0

            Root

         /   |   \

Level 1

       A     B     C

     / | \

Level 2

   D  E  F

  / \

Level 3

 G   H

...
```

Each level assigns

```
One Worker
```

---

# 9. Bounding Mechanism

Bounding is the heart of Branch and Bound.

Without bounding,

the algorithm becomes

```
Brute Force
```

---

## Lower Bound

The lower bound estimates

```
Minimum Possible Cost
```

reachable from the current node.

Formula

```
Lower Bound

=

Cost Already Incurred

+

Minimum Possible Remaining Cost
```

---

## Visualization

```
Current Cost

      12

Remaining Minimum

       8

------------------

Lower Bound

      20
```

Suppose current best solution

```
18
```

Since

```
20 > 18
```

This branch

```
Cannot Improve

↓

Prune
```

---

## Pruning Illustration

```
                Root

           /      |      \

         15      22      11

        / \

      18  25

Current Best = 16

Node 22

↓

Pruned

Node 25

↓

Pruned
```

Only

```
11

and

15
```

continue.

---

# 10. Branching Strategy

Each level assigns

```
Exactly One Worker
```

Example

```
Worker 1

↓

Job1

Job2

Job3

Job4
```

Each assignment creates a child node.

---

Example

```
Root

|

Worker1

|

+-----------------------+

|        |        |     |

J1       J2      J3    J4
```

Each child corresponds to one possible assignment.

Used jobs cannot be selected again.

---

# 11. Search Strategy

The algorithm always expands

```
Node with

Minimum Lower Bound
```

This is called

```
Best First Search
```

Visualization

```
Priority Queue

---------------------

Node A  LB=13

Node B  LB=18

Node C  LB=10

Node D  LB=25

---------------------

Expand

↓

Node C
```

The priority queue always removes

```
Lowest Bound First
```

---

# 12. Cost Matrix

Example

```
          Jobs

          J1 J2 J3 J4

W1        9  2  7  8

W2        6  4  3  7

W3        5  8  1  8

W4        7  6  9  4
```

Interpretation

```
Assign W1 → J2

Cost = 2

Assign W4 → J4

Cost = 4
```

The objective

```
Choose one element

from every row

and every column

having minimum total cost.
```

---

# 13. Lower Bound Calculation

Suppose

```
Worker1

↓

Job2
```

Cost

```
2
```

Remaining workers

```
W2

W3

W4
```

Choose the minimum available value from each remaining row (ignoring already assigned columns):

```
W2 → 3

W3 → 1

W4 → 4
```

Estimated lower bound:

```
Current Cost

2

+

Remaining Minimum

3 + 1 + 4

=

10
```

ASCII representation:

```
Current Assignment

W1 → J2

Cost = 2

Remaining Estimate

W2 → 3

W3 → 1

W4 → 4

--------------------

Lower Bound = 10
```

This lower bound helps determine whether expanding this node is worthwhile.

---

**End of Part 1**

**Next Part (Part 2) includes:**
- Complete Branch and Bound algorithm
- Detailed pseudocode
- Full step-by-step worked example
- Complete state-space tree
- Bounding and pruning walkthrough
- ASCII flowcharts
- Priority queue evolution

---

# 14. Branch and Bound Algorithm

The Branch and Bound algorithm systematically explores the state-space tree while eliminating branches that cannot produce a better solution than the best one found so far.

The key idea is:

```
Expand only the most promising node.

↓

Compute its lower bound.

↓

Prune if necessary.

↓

Repeat until a complete assignment is found.
```

---

## Overall Algorithm

```
Start

↓

Create Root Node

↓

Calculate Lower Bound

↓

Insert into Priority Queue

↓

While Queue is Not Empty

↓

Remove Node with Lowest Bound

↓

Is it a Complete Assignment?

      |

     Yes ----------------------> Update Best Solution

      |

     No

↓

Generate Child Nodes

↓

Compute Lower Bound

↓

Prune Bad Nodes

↓

Insert Remaining Nodes

↓

Repeat

↓

Finish
```

---

# 15. Step-by-Step Procedure

## Step 1

Create the root node.

```
Assignments

None
```

```
Current Cost = 0
```

Calculate its lower bound.

---

## Step 2

Insert the root node into a priority queue.

```
Priority Queue

----------------

Root (LB = ?)

----------------
```

---

## Step 3

Remove the node having the smallest lower bound.

```
Priority Queue

---------------

A (10)

B (15)

C (22)

---------------

Expand

↓

A
```

---

## Step 4

Assign the next worker to every available job.

Example

```
Worker 1

↓

J1

J2

J3

J4
```

Four child nodes are generated.

---

## Step 5

For every child

Calculate

```
Current Cost

+

Estimated Remaining Cost

=

Lower Bound
```

Example

```
Child

W1 → J2

Current Cost = 2

Remaining Estimate = 8

Lower Bound = 10
```

---

## Step 6

Compare with the current best solution.

```
Lower Bound = 25

Current Best = 18

25 > 18

↓

Discard
```

---

## Step 7

Insert promising nodes back into the priority queue.

```
Queue

-------------

LB=10

LB=14

LB=16

-------------
```

---

## Step 8

Continue until a leaf node is reached.

```
Every Worker Assigned

↓

Optimal Solution
```

---

# 16. Pseudocode

```text
Create Root Node

Compute Root Lower Bound

Insert Root into Priority Queue

Best Cost = ∞

while Queue is not Empty

    Remove Node with Smallest Lower Bound

    if Node is Complete Assignment

         Update Best Cost

    else

         Generate Children

         for each Child

              Compute Lower Bound

              if Lower Bound < Best Cost

                    Insert into Queue

return Best Assignment
```

---

# 17. Worked Example

Consider the following cost matrix.

```
            Jobs

          J1  J2  J3  J4

W1         9   2   7   8

W2         6   4   3   7

W3         5   8   1   8

W4         7   6   9   4
```

Goal

```
Minimum Cost Assignment
```

---

## Root Node

Assignments

```
None
```

```
Cost = 0
```

Choose minimum value from every row.

```
Row1 = 2

Row2 = 3

Row3 = 1

Row4 = 4
```

Lower Bound

```
0 + 2 + 3 + 1 + 4

=

10
```

Priority Queue

```
Root

LB = 10
```

---

# 18. First Expansion

Expand the root.

Assign Worker 1.

Possible children

```
             Root

       /      |      |      \

      J1     J2     J3      J4
```

---

## Child 1

```
W1 → J1
```

Current Cost

```
9
```

Remaining minimum estimate

```
3 + 1 + 4 = 8
```

Lower Bound

```
17
```

---

## Child 2

```
W1 → J2
```

Current Cost

```
2
```

Remaining estimate

```
3 + 1 + 4

=

8
```

Lower Bound

```
10
```

---

## Child 3

```
W1 → J3
```

Current Cost

```
7
```

Remaining estimate

```
4 + 5 + 4

=

13
```

Lower Bound

```
20
```

---

## Child 4

```
W1 → J4
```

Current Cost

```
8
```

Remaining estimate

```
3 + 1 + 6

=

10
```

Lower Bound

```
18
```

---

## Queue after Expansion

```
--------------------------------

Node             Lower Bound

--------------------------------

W1→J2               10

W1→J1               17

W1→J4               18

W1→J3               20

--------------------------------
```

The algorithm expands

```
W1 → J2
```

because

```
LB = 10
```

is minimum.

---

# 19. Second Expansion

Current assignment

```
W1 → J2
```

Assigned job

```
J2
```

Available jobs

```
J1

J3

J4
```

Generate children.

```
                W1→J2

          /        |        \

        J1        J3        J4
```

---

## Child A

```
W2 → J1
```

Assignments

```
W1 → J2

W2 → J1
```

Current Cost

```
2 + 6 = 8
```

Remaining estimate

```
1 + 4

=

5
```

Lower Bound

```
13
```

---

## Child B

```
W2 → J3
```

Assignments

```
W1 → J2

W2 → J3
```

Current Cost

```
2 + 3

=

5
```

Remaining estimate

```
5 + 4

=

9
```

Lower Bound

```
14
```

---

## Child C

```
W2 → J4
```

Assignments

```
W1 → J2

W2 → J4
```

Current Cost

```
9
```

Remaining estimate

```
1 + 7

=

8
```

Lower Bound

```
17
```

---

## Updated Queue

```
---------------------------------

Node                 LB

---------------------------------

W1→J2→J1            13

W1→J2→J3            14

W1→J1               17

W1→J2→J4            17

W1→J4               18

W1→J3               20

---------------------------------
```

Expand

```
LB = 13
```

---

# 20. State-Space Tree Walkthrough

```
                              Root
                            LB=10
                               |
      -------------------------------------------------
      |               |              |               |
   W1→J1          W1→J2          W1→J3          W1→J4
   LB17           LB10           LB20           LB18
                    |
          -----------------------
          |          |          |
      W2→J1      W2→J3      W2→J4
      LB13       LB14       LB17
          |
     ----------------
     |              |
  W3→J3         W3→J4
```

Every level

```
Assigns one worker.
```

---

# 21. Bounding Mechanism

Suppose

Current Best Solution

```
16
```

Current queue

```
Node A

LB = 13

Node B

LB = 18

Node C

LB = 21

Node D

LB = 14
```

Visualization

```
                Queue

      +----------------------+

      | LB 13   Expand       |

      | LB 14               |

      | LB 18   Prune Later |

      | LB 21   Prune Later |

      +----------------------+
```

Only nodes capable of improving the best solution are explored.

---

# 22. Pruning Example

Suppose

```
Best Cost = 15
```

Current node

```
Current Cost = 12

Remaining Minimum = 6
```

Lower Bound

```
18
```

Since

```
18 > 15
```

the entire subtree is discarded.

Visualization

```
                   Node

                LB = 18

               /   |   \

             A     B     C

            /|\   /|\   /|\

All Ignored

Reason

↓

Cannot Beat Best Cost
```

---

# 23. Priority Queue Evolution

Initially

```
[ Root (10) ]
```

Expand Root

```
[10]

↓

17

10

20

18
```

Remove minimum

```
17

20

18

↓

13

14

17
```

Remove minimum again

```
14

17

17

18

20
```

The smallest lower bound always has the highest priority.

---

# 24. Best-First Search Visualization

```
                 Root

               LB =10

            /    |    \

         17      10      20

                 |

               13

               |

              12

              |

             Goal
```

Expansion order

```
10

↓

13

↓

12

↓

Goal
```

Notice that nodes with lower bounds of **17**, **18**, and **20** are postponed until all more promising nodes have been explored.

---

# 25. Complete Flowchart

```text
                START
                   |
                   v
          Read Cost Matrix
                   |
                   v
          Create Root Node
                   |
                   v
      Compute Root Lower Bound
                   |
                   v
      Insert into Priority Queue
                   |
                   v
       +----------------------+
       | Queue Empty ?        |
       +----------------------+
          |             |
         Yes           No
          |             |
          v             v
        STOP     Remove Best Node
                        |
                        v
            Complete Assignment?
                  |         |
                 Yes        No
                  |         |
                  v         v
        Update Best Cost   Generate Children
                  |         |
                  |         v
                  |   Compute Lower Bounds
                  |         |
                  |         v
                  |   Prune Bad Nodes
                  |         |
                  +---------+
                            |
                            v
                Insert Remaining Nodes
                            |
                            +----> Repeat
```

---

## Key Points for Exams

- Branch and Bound guarantees the **optimal solution**.
- It uses **Best-First Search** with a **Priority Queue**.
- Every node stores:
  - Partial assignment
  - Current cost
  - Lower bound
  - Assigned jobs
- The **lower bound** determines whether a branch is expanded or pruned.
- Pruning significantly reduces the number of explored states compared to brute force.

---

**End of Part 2**

**Part 3 includes:**
- Complete Java implementation (fully commented)
- Node class explanation
- Priority Queue implementation
- Complete dry run of the Java code
- Time and space complexity analysis
- Correctness proof

---

# 26. Complete Java Implementation

The following Java program implements the **Branch and Bound algorithm for the Assignment Problem** using a **Best-First Search** strategy with a **Priority Queue (Min-Heap)**.

> **Note:** This implementation is intended for learning and semester exam preparation. It clearly demonstrates branching, bounding, and pruning.

```java
import java.util.Arrays;
import java.util.PriorityQueue;

public class AssignmentBranchAndBound {

    // Represents a node in the state-space tree
    static class Node implements Comparable<Node> {

        Node parent;          // Parent node
        int[][] path;         // Assigned worker-job pairs
        boolean[] assigned;   // Assigned jobs
        int level;            // Current worker index
        int cost;             // Cost accumulated so far
        int lowerBound;       // Estimated minimum total cost

        Node(Node parent, int[][] path,
             boolean[] assigned,
             int level,
             int cost,
             int lowerBound) {

            this.parent = parent;
            this.path = path;
            this.assigned = assigned;
            this.level = level;
            this.cost = cost;
            this.lowerBound = lowerBound;
        }

        @Override
        public int compareTo(Node other) {
            return this.lowerBound - other.lowerBound;
        }
    }

    // Computes lower bound
    static int calculateLowerBound(int[][] costMatrix,
                                   int level,
                                   boolean[] assignedJobs,
                                   int currentCost) {

        int bound = currentCost;
        int n = costMatrix.length;

        for (int worker = level + 1; worker < n; worker++) {

            int min = Integer.MAX_VALUE;

            for (int job = 0; job < n; job++) {

                if (!assignedJobs[job]) {
                    min = Math.min(min, costMatrix[worker][job]);
                }
            }

            bound += min;
        }

        return bound;
    }

    // Solves Assignment Problem
    static void solve(int[][] costMatrix) {

        int n = costMatrix.length;

        PriorityQueue<Node> pq = new PriorityQueue<>();

        boolean[] assigned = new boolean[n];

        int[][] path = new int[n][2];

        int rootBound = calculateLowerBound(
                costMatrix,
                -1,
                assigned,
                0);

        Node root = new Node(
                null,
                path,
                assigned,
                -1,
                0,
                rootBound);

        pq.add(root);

        int bestCost = Integer.MAX_VALUE;

        Node bestNode = null;

        while (!pq.isEmpty()) {

            Node current = pq.poll();

            if (current.lowerBound >= bestCost)
                continue;

            int worker = current.level + 1;

            if (worker == n) {

                bestCost = current.cost;
                bestNode = current;
                continue;
            }

            for (int job = 0; job < n; job++) {

                if (!current.assigned[job]) {

                    boolean[] newAssigned =
                            Arrays.copyOf(
                                    current.assigned,
                                    n);

                    newAssigned[job] = true;

                    int[][] newPath =
                            new int[n][2];

                    for (int i = 0; i < worker; i++)
                        newPath[i] = Arrays.copyOf(
                                current.path[i], 2);

                    newPath[worker][0] = worker;
                    newPath[worker][1] = job;

                    int newCost =
                            current.cost +
                                    costMatrix[worker][job];

                    int bound =
                            calculateLowerBound(
                                    costMatrix,
                                    worker,
                                    newAssigned,
                                    newCost);

                    Node child =
                            new Node(
                                    current,
                                    newPath,
                                    newAssigned,
                                    worker,
                                    newCost,
                                    bound);

                    pq.add(child);
                }
            }
        }

        System.out.println("Optimal Cost : " + bestCost);

        System.out.println("\nAssignments");

        for (int i = 0; i < n; i++) {

            System.out.println(
                    "Worker "
                            + (bestNode.path[i][0] + 1)
                            + " --> Job "
                            + (bestNode.path[i][1] + 1));
        }
    }

    public static void main(String[] args) {

        int[][] costMatrix = {

                {9,2,7,8},

                {6,4,3,7},

                {5,8,1,8},

                {7,6,9,4}
        };

        solve(costMatrix);
    }
}
```

---

# 27. Code Explanation

## Step 1 — Node Class

Every node represents a **partial assignment**.

```
Node

↓

Current Worker

↓

Assigned Jobs

↓

Current Cost

↓

Lower Bound
```

Visualization

```
+----------------------+

Worker Level

Current Cost

Lower Bound

Assigned Jobs

Path

+----------------------+
```

---

## Step 2 — Priority Queue

```java
PriorityQueue<Node> pq
```

stores live nodes.

Since `Node` implements `Comparable`, the queue automatically keeps the node with the **smallest lower bound** at the top.

```
Priority Queue

-------------------

LB = 10

LB = 13

LB = 15

LB = 18

-------------------
```

The node with `LB = 10` is expanded first.

---

## Step 3 — Lower Bound Function

Purpose

```
Estimate

Minimum Possible Cost
```

Formula

```
Current Cost

+

Minimum Remaining Cost
```

Example

```
Current Cost

5

Remaining

1 + 4

=

5

Bound

=

10
```

---

## Step 4 — Branching

Suppose

```
Worker 1
```

Possible assignments

```
Worker1

↓

J1

J2

J3

J4
```

Four children are generated.

---

## Step 5 — Pruning

Suppose

```
Best Cost

=

15
```

Current node

```
Lower Bound

=

18
```

```
18 > 15
```

Therefore

```
Discard Entire Subtree
```

Visualization

```
             Node

           LB = 18

         /   |   \

       A     B     C

Ignored
```

---

# 28. Dry Run

Consider

```
          J1 J2 J3 J4

W1        9  2  7  8

W2        6  4  3  7

W3        5  8  1  8

W4        7  6  9  4
```

---

## Initial Step

```
Current Cost

0
```

Minimum values

```
2

3

1

4
```

Root bound

```
10
```

Queue

```
Root

LB=10
```

---

## Expand Root

Generated nodes

```
W1→J1

LB=17

------------

W1→J2

LB=10

------------

W1→J3

LB=20

------------

W1→J4

LB=18
```

Minimum

```
10
```

Expand

```
W1→J2
```

---

## Expand Again

Assignments

```
W1→J2

↓

Worker2
```

Possible jobs

```
J1

J3

J4
```

Generated

```
LB13

LB14

LB17
```

Expand

```
LB13
```

---

## Continue

The process repeats until

```
Level = n
```

which means

```
Complete Assignment
```

---

## Final Output

```
Optimal Cost = 13

Assignments

Worker1 → Job2

Worker2 → Job1

Worker3 → Job3

Worker4 → Job4
```

---

# 29. Correctness

Why does the algorithm always produce the optimal solution?

### Reason 1

Every feasible assignment exists somewhere in the state-space tree.

```
Root

↓

Children

↓

Grandchildren

↓

Leaf Nodes
```

Every leaf represents one valid assignment.

---

### Reason 2

The lower bound never **underestimates** the minimum possible cost from a node.

Therefore,

if

```
Lower Bound

>

Current Best
```

the node cannot improve the answer.

Pruning is safe.

---

### Reason 3

The priority queue always expands the node with the **smallest lower bound**, ensuring the search focuses on the most promising partial assignments first.

---

# 30. Time Complexity

Worst case

```
O(n!)
```

because every assignment may still need to be explored.

However,

effective pruning greatly reduces the number of explored nodes in practice.

---

## Best Case

```
Much smaller than n!
```

If many branches are pruned early.

---

## Average Case

Depends on

- Cost distribution
- Quality of lower bound
- Number of pruned branches

---

### Complexity Table

| Case | Time Complexity |
|------|-----------------|
| Best | Much less than `O(n!)` |
| Average | Better than brute force |
| Worst | `O(n!)` |

---

# 31. Space Complexity

The algorithm stores

- Priority queue
- State-space nodes
- Assignment paths
- Assigned job arrays

Worst-case space

```
O(n!)
```

due to the number of live nodes.

---

# 32. Advantages

### 1. Optimal Solution

Always finds the minimum-cost assignment.

---

### 2. Intelligent Search

Explores only promising nodes.

```
Brute Force

↓

Everything

Branch and Bound

↓

Only Useful Nodes
```

---

### 3. Significant Pruning

Large portions of the tree are skipped.

```
          Root

       /    |    \

      ✓     ✗     ✓

           Pruned
```

---

### 4. Faster Than Brute Force

Especially for medium and large problem sizes.

---

### 5. General Technique

Applicable to many optimization problems such as:

- Assignment Problem
- Traveling Salesman Problem
- 15-Puzzle
- 0/1 Knapsack
- Job Scheduling

---

# 33. Disadvantages

### 1. Worst-Case Complexity

Still exponential.

```
O(n!)
```

---

### 2. Memory Usage

Priority queue can become large.

---

### 3. Depends on Bounding Function

A weak lower bound results in little pruning.

```
Poor Bound

↓

More Nodes

↓

Slower Execution
```

---

### 4. Implementation Complexity

More difficult than brute force or greedy algorithms because of:

- Node management
- Priority queue
- Bound computation
- Pruning logic

---

# 34. Exam Tips

Remember the following points:

- Branch and Bound is an **exact optimization technique**.
- It explores the **state-space tree**.
- Each node stores a **partial assignment**.
- The **lower bound** estimates the minimum achievable cost.
- Nodes whose lower bound is not competitive are **pruned**.
- A **Priority Queue (Min-Heap)** implements Best-First Search.
- The algorithm guarantees the **optimal assignment**.

---

**End of Part 3**

**Part 4 includes:**
- Comparison with Greedy, Dynamic Programming, Hungarian Algorithm, and Brute Force
- Limitations with ASCII illustrations
- Edge cases
- Frequently asked viva questions
- Semester exam questions
- One-page revision sheet
- Conclusion

  ---

# 35. Comparison with Other Approaches

The Assignment Problem can be solved using several methods. Each method has different strengths and weaknesses.

## Comparison Table

| Feature | Branch & Bound | Greedy | Dynamic Programming | Brute Force | Hungarian Algorithm |
|----------|----------------|---------|---------------------|-------------|---------------------|
| Guarantees Optimal Solution | ✅ Yes | ❌ No | ✅ Yes | ✅ Yes | ✅ Yes |
| Uses Pruning | ✅ Yes | ❌ No | ❌ No | ❌ No | ❌ No |
| Time Complexity | Worst: `O(n!)` | `O(n²)` (approx.) | `O(n²·2ⁿ)` (Bitmask DP) | `O(n!)` | `O(n³)` |
| Practical Performance | Good | Very Fast | Good for small n | Poor | Excellent |
| Memory Usage | Moderate–High | Low | High | Low | Moderate |
| Best For | General optimization | Fast approximations | Small constraints | Learning | Assignment problems |

---

## Visual Comparison

```
Greedy

Choose Local Best

↓

Fast

↓

May Miss Optimum



Branch and Bound

Explore Promising Nodes

↓

Prune Bad Branches

↓

Optimal Solution



Dynamic Programming

Store Subproblems

↓

Reuse Solutions

↓

Optimal



Brute Force

Try Every Assignment

↓

Huge Search

↓

Optimal but Slow



Hungarian Algorithm

Matrix Reduction

↓

Optimal Matching

↓

Best Practical Algorithm
```

---

# 36. Branch and Bound vs Brute Force

## Brute Force

```
                Root

        / / / / | \ \ \ \

Explore Every Branch

↓

Every Leaf

↓

Find Minimum
```

Nodes visited

```
100%
```

---

## Branch and Bound

```
               Root

         /      |      \

      Good     Bad     Good

       |        X         |

      Good      X       Better

       |        X         |

     Solution
```

Nodes visited

```
Only promising branches
```

---

## Summary

| Brute Force | Branch & Bound |
|-------------|----------------|
| Examines every solution | Skips unnecessary solutions |
| No pruning | Uses pruning |
| Slow | Faster in practice |
| Very simple | More complex |
| Optimal | Optimal |

---

# 37. Branch and Bound vs Greedy

## Greedy Strategy

```
Worker 1

↓

Choose Cheapest Job

↓

Worker 2

↓

Choose Cheapest Remaining

↓

Continue
```

Problem

```
Local Minimum

≠

Global Minimum
```

---

### Example

```
      J1  J2

W1     2   3

W2     1 100
```

Greedy

```
W1 → J1 = 2

W2 → J2 =100

Total =102
```

Optimal

```
W1 → J2 =3

W2 → J1 =1

Total =4
```

Greedy fails because the first local choice blocks a much better overall assignment.

---

# 38. Branch and Bound vs Dynamic Programming

Dynamic Programming stores solutions to overlapping subproblems.

```
Problem

↓

Subproblems

↓

Store Results

↓

Reuse Later
```

Branch and Bound

```
Problem

↓

Tree Search

↓

Bounding

↓

Pruning
```

---

## Difference

Dynamic Programming

```
Avoids repeated work.
```

Branch and Bound

```
Avoids exploring impossible solutions.
```

---

# 39. Hungarian Algorithm vs Branch and Bound

The Hungarian Algorithm is specifically designed for the Assignment Problem.

```
Cost Matrix

↓

Row Reduction

↓

Column Reduction

↓

Optimal Matching
```

Branch and Bound

```
State Space Tree

↓

Lower Bounds

↓

Pruning

↓

Optimal Assignment
```

---

## Which is Better?

### Hungarian Algorithm

- Faster
- Polynomial time
- Best choice for pure Assignment Problems

### Branch and Bound

- More general
- Applicable to many optimization problems
- Easier to extend with constraints

---

# 40. Limitations

Although Branch and Bound is powerful, it has several limitations.

---

## Limitation 1

### Worst-Case Exponential Time

```
Root

↓

Almost No Pruning

↓

Entire Tree

↓

O(n!)
```

If the lower bound is weak, almost every node must be explored.

---

## Limitation 2

### High Memory Usage

Many live nodes may exist simultaneously.

```
Priority Queue

+-------------------+

Node

Node

Node

Node

Node

Node

...

+-------------------+
```

---

## Limitation 3

### Lower Bound Quality Matters

```
Weak Bound

↓

Less Pruning

↓

Large Search Tree
```

```
Strong Bound

↓

More Pruning

↓

Fast Solution
```

---

## Limitation 4

### Difficult Implementation

Requires

- Priority Queue
- State-space tree
- Bound calculation
- Pruning logic
- Node management

---

# 41. Edge Cases

---

## Edge Case 1

### Single Worker

```
      J1

W1     7
```

Only one assignment exists.

```
Answer = 7
```

---

## Edge Case 2

### Equal Costs

```
      J1 J2 J3

W1     5  5  5

W2     5  5  5

W3     5  5  5
```

Many optimal assignments exist.

---

## Edge Case 3

### Zero Costs

```
      J1 J2

W1     0  6

W2     4  0
```

Optimal assignment

```
W1 → J1

W2 → J2
```

Total

```
0
```

---

## Edge Case 4

### Large Costs

```
      J1   J2

W1   999 1000

W2   998  997
```

Algorithm still works correctly because it compares relative costs.

---

## Edge Case 5

### Multiple Optimal Solutions

```
      J1 J2

W1     1  1

W2     1  1
```

Possible optimal assignments

```
W1→J1

W2→J2
```

or

```
W1→J2

W2→J1
```

Both have total cost

```
2
```

---

# 42. Frequently Asked Viva Questions

### Q1. What is the Assignment Problem?

Assign each worker to exactly one job such that the total cost is minimum.

---

### Q2. Why is Branch and Bound used?

To avoid exploring unnecessary solutions while still guaranteeing the optimal answer.

---

### Q3. What is branching?

Generating child nodes by making one additional assignment.

---

### Q4. What is bounding?

Computing a lower bound on the minimum possible cost from a partial assignment.

---

### Q5. What is pruning?

Discarding branches that cannot improve the current best solution.

---

### Q6. Which data structure is commonly used?

A **Priority Queue (Min-Heap)**.

---

### Q7. Which search strategy is used?

**Best-First Search** based on the smallest lower bound.

---

### Q8. Is the solution always optimal?

Yes.

---

### Q9. What is the worst-case time complexity?

```
O(n!)
```

---

### Q10. What is the purpose of the lower bound?

To estimate the minimum possible cost from a node and decide whether it should be explored.

---

# 43. Semester Examination Questions

## Short Questions (2 Marks)

- Define the Assignment Problem.
- Define Branch and Bound.
- What is a lower bound?
- What is pruning?
- What is a live node?
- What is a dead node?
- Define a state-space tree.
- What is a priority queue?

---

## Medium Questions (5 Marks)

- Explain the Branch and Bound strategy.
- Explain the bounding mechanism.
- Explain pruning with an example.
- Draw a state-space tree for a small assignment problem.
- Compare Branch and Bound with Greedy.

---

## Long Questions (10–15 Marks)

- Explain the Assignment Problem using Branch and Bound with a suitable example.
- Explain the algorithm with a state-space tree.
- Write the Branch and Bound algorithm and explain its working.
- Write a Java program to solve the Assignment Problem using Branch and Bound.
- Compare Branch and Bound with Hungarian Algorithm and Brute Force.

---

# 44. One-Page Revision Sheet

## Remember

```
Assignment Problem

↓

Optimization

↓

Minimum Cost

↓

One Worker

↓

One Job
```

---

### Core Idea

```
State Space Tree

↓

Lower Bound

↓

Priority Queue

↓

Best First Search

↓

Pruning

↓

Optimal Solution
```

---

### Formula

```
Lower Bound

=

Current Cost

+

Minimum Remaining Cost
```

---

### Node Contains

```
Worker Level

Current Cost

Lower Bound

Assigned Jobs

Assignment Path
```

---

### Pruning Rule

```
If

Lower Bound

≥

Best Cost

↓

Prune Node
```

---

### Complexity

| Complexity | Value |
|------------|-------|
| Best Case | Much less than `O(n!)` |
| Average Case | Better than Brute Force |
| Worst Case | `O(n!)` |
| Space | Up to `O(n!)` |

---

### Advantages

- Optimal solution
- Intelligent pruning
- Faster than brute force
- General optimization technique

---

### Disadvantages

- Worst-case exponential
- High memory usage
- Depends on lower bound quality
- Complex implementation

---

# 45. Key Takeaways

1. The Assignment Problem is an optimization problem where each worker is assigned exactly one job.
2. Branch and Bound searches a **state-space tree** of partial assignments.
3. Every node stores the current assignment, accumulated cost, and a lower bound.
4. The **lower bound** estimates the minimum possible cost that can still be achieved.
5. Nodes whose lower bound cannot beat the current best solution are **pruned**.
6. A **Priority Queue** implements **Best-First Search**, always expanding the node with the smallest lower bound.
7. Branch and Bound guarantees the **optimal solution**, but its worst-case complexity remains exponential.
8. For the pure Assignment Problem, the **Hungarian Algorithm (`O(n³)`)** is generally more efficient, while Branch and Bound is valuable because the same strategy applies to many other combinatorial optimization problems.

---

# 46. Conclusion

Branch and Bound is one of the most important exact techniques for solving combinatorial optimization problems. For the Assignment Problem, it improves significantly over brute force by using **lower bounds** to eliminate unpromising branches while still guaranteeing the optimal solution.

Although the worst-case complexity is still exponential, practical performance is often much better because many branches are pruned before reaching complete assignments. Understanding **state-space trees**, **branching**, **bounding**, **pruning**, and **Best-First Search** is essential for both semester examinations and interviews involving algorithm design.

---

# Quick Memory Trick

```
Assignment Problem

↓

Branch

(Create Choices)

↓

Bound

(Estimate Minimum Cost)

↓

Prune

(Removes Bad Choices)

↓

Priority Queue

(Expand Best Node)

↓

Optimal Assignment
```

---

**End of Complete Study Guide**
