# Graph Coloring using Backtracking
### Design and Analysis of Algorithms (DAA) – Complete Study Notes

> **Topic:** Graph Coloring Problem using Backtracking  
> **Algorithm Paradigm:** Backtracking  
> **Difficulty:** Medium–Hard  
> **Applications:** Scheduling, Register Allocation, Frequency Assignment, Timetable Generation, Map Coloring, Network Optimization

---

# Table of Contents

- [1. Introduction](#1-introduction)
- [2. What is Graph Coloring?](#2-what-is-graph-coloring)
- [3. Basic Graph Terminology](#3-basic-graph-terminology)
- [4. Why Graph Coloring is Needed](#4-why-graph-coloring-is-needed)
- [5. Real-World Applications](#5-real-world-applications)
- [6. Why Backtracking?](#6-why-backtracking)
- [7. Problem Statement](#7-problem-statement)
- [8. Mathematical Formulation](#8-mathematical-formulation)
- [9. Constraints of the Problem](#9-constraints-of-the-problem)
- [10. Example Graph](#10-example-graph)
- [11. Understanding Coloring Rules](#11-understanding-coloring-rules)
- [12. Chromatic Number](#12-chromatic-number)
- [13. Types of Graph Coloring Problems](#13-types-of-graph-coloring-problems)
- [14. Key Properties](#14-key-properties)
- [15. Relationship with NP-Complete Problems](#15-relationship-with-np-complete-problems)

---

# 1. Introduction

Graph Coloring is one of the most important combinatorial optimization problems in computer science and graph theory.

The objective is simple:

> Assign colors to the vertices of a graph so that **no two adjacent vertices have the same color.**

Although the problem statement is simple, finding a valid coloring using the minimum number of colors becomes computationally difficult as the graph grows.

Because of this difficulty, Graph Coloring is a classic example used to explain **Backtracking**.

Backtracking systematically explores all possible color assignments while abandoning choices that violate the coloring constraints.

It is an excellent demonstration of:

- Constraint Satisfaction Problems (CSP)
- State Space Search
- Recursive Algorithms
- NP-Complete Problems

---

# 2. What is Graph Coloring?

Graph coloring is the process of assigning colors to vertices of a graph such that adjacent vertices receive different colors.

Suppose the graph is

```
A ----- B
|       |
|       |
C ----- D
```

If A is Red,

then B cannot be Red.

If C is adjacent to A,

then C also cannot be Red.

One valid coloring is

```
A = Red
B = Blue
C = Blue
D = Red
```

Visual representation

```
          (Red)
             A
           /   \
          /     \
 (Blue) B-------D (Red)
          \     /
           \   /
          (Blue)
             C
```

Observe that:

- Every adjacent pair has different colors.
- Non-adjacent vertices may share colors.

---

# 3. Basic Graph Terminology

Before understanding Graph Coloring, let's review some important graph concepts.

---

## Vertex (Node)

A point in a graph.

```
•
```

Example

```
A
```

---

## Edge

Connection between two vertices.

```
A ------- B
```

---

## Adjacent Vertices

Vertices connected directly by an edge.

```
A ------- B
```

A and B are adjacent.

---

## Degree

Number of edges connected to a vertex.

Example

```
      B
      |
A ----C----D
      |
      E
```

Degree(C) = 4

---

## Complete Graph

Every vertex is connected to every other vertex.

Example (K4)

```
A------B
|\    /|
| \  / |
|  \/  |
|  /\  |
| /  \ |
|/    \|
C------D
```

Every vertex connects to all others.

---

## Empty Graph

No edges.

```
A    B    C    D
```

No restrictions on colors.

---

## Bipartite Graph

Vertices are divided into two sets.

Edges exist only between sets.

```
A      B
|\    /|
| \  / |
|  \/  |
|  /\  |
| /  \ |
|/    \|
C      D
```

Can always be colored with only **2 colors**.

---

# 4. Why Graph Coloring is Needed

Many practical problems involve assigning limited resources while avoiding conflicts.

Graph Coloring models these conflicts naturally.

General idea:

```
Resource  ←→ Color

Conflict ←→ Edge
```

Whenever two tasks conflict,

they are connected by an edge.

Different colors represent different resource assignments.

---

## Example

Suppose two exams are attended by the same student.

```
Math -------- Physics
```

These exams cannot be held at the same time.

Assign

```
Morning = Red

Afternoon = Blue
```

Then

```
Math → Red

Physics → Blue
```

No conflict occurs.

---

# 5. Real-World Applications

Graph Coloring appears in many engineering fields.

---

## 1. University Timetable Scheduling

Courses become vertices.

An edge exists if students take both courses.

```
Math ------- Physics
   \
    \
   Chemistry
```

Colors represent time slots.

```
Red    → Monday

Blue   → Tuesday

Green  → Wednesday
```

---

## 2. Register Allocation in Compilers

Variables that are simultaneously active cannot use the same CPU register.

Conflict graph

```
x ----- y
 \     /
  \   /
    z
```

Registers become colors.

```
Register 1

Register 2

Register 3
```

Compiler assigns registers through graph coloring.

---

## 3. Frequency Assignment

Nearby cellular towers cannot use identical frequencies.

```
Tower A ----- Tower B
```

Frequency channels become colors.

---

## 4. Map Coloring

Countries sharing borders cannot use identical colors.

Example

```
+-----+-----+
|  A  |  B  |
+-----+-----+
|  C  |  D  |
+-----+-----+
```

Adjacent regions receive different colors.

---

## 5. Wireless Network Channel Assignment

Nearby Wi-Fi routers interfere if configured to identical channels.

```
Router1 ----- Router2
     \
      \
     Router3
```

Channels become colors.

---

## 6. Sports Scheduling

Teams sharing stadiums or players cannot play simultaneously.

Conflict graph determines match schedule.

---

## 7. PCB Design

Electronic components producing interference are modeled using graph coloring.

---

## 8. Task Scheduling

Tasks requiring identical resources cannot execute simultaneously.

Colors represent execution slots.

---

# 6. Why Backtracking?

Suppose only three colors are available.

```
Red

Blue

Green
```

Graph

```
A ----- B
|       |
|       |
C ----- D
```

Coloring begins.

```
A = Red
```

Next,

```
B = Blue
```

Next,

```
C = Blue
```

Now color D.

```
D cannot be Red

D cannot be Blue
```

Try Green.

If Green also violates another constraint in a larger graph,

the algorithm cannot continue.

Instead of restarting from scratch,

Backtracking simply returns to the previous decision.

```
A = Red

↓

B = Blue

↓

C = Blue

↓

D = ❌

↓

Backtrack

↓

Change C

↓

Continue
```

This saves enormous computation compared to brute force.

---

## Visual Idea

```
           Start

              |

          Color A

              |

          Color B

              |

          Color C

              |

        Color D fails

              |

        ← Backtrack

              |

      Try another color

              |

         Continue Search
```

---

# 7. Problem Statement

Given

- A graph **G(V,E)**
- **m** available colors

Determine whether it is possible to color every vertex so that

- Adjacent vertices receive different colors.

If possible,

output one valid coloring.

Otherwise,

report that no solution exists.

---

## Input

Example adjacency matrix

```
      A B C D

A     0 1 1 0

B     1 0 1 1

C     1 1 0 1

D     0 1 1 0
```

Available colors

```
m = 3
```

---

## Expected Output

```
A → Color 1

B → Color 2

C → Color 3

D → Color 1
```

---

# 8. Mathematical Formulation

Let

```
Graph

G = (V,E)
```

where

```
V = Vertices

E = Edges
```

Let

```
Colors = {1,2,...,m}
```

Find a function

```
f : V → {1,2,...,m}
```

such that

```
For every edge (u,v),

f(u) ≠ f(v)
```

This is the formal definition of the graph coloring problem.

---

# 9. Constraints of the Problem

The coloring must satisfy:

### Constraint 1

Every vertex receives exactly one color.

```
✓ Allowed

A = Red
```

```
✗ Not Allowed

A = Red

A = Blue
```

---

### Constraint 2

Adjacent vertices cannot share colors.

```
A ----- B
```

Allowed

```
Red

Blue
```

Not Allowed

```
Red

Red
```

---

### Constraint 3

Maximum available colors = m

Example

```
m = 3
```

Allowed

```
Red

Blue

Green
```

Not Allowed

```
Yellow
```

---

# 10. Example Graph

Consider

```
        A
      /   \
     /     \
    B-------C
     \     /
      \   /
        D
```

Adjacency list

```
A : B C

B : A C D

C : A B D

D : B C
```

Suppose

```
m = 3
```

One possible coloring

```
A → Red

B → Blue

C → Green

D → Red
```

Visualization

```
         (Red)
            A
          /   \
         /     \
 (Blue) B-------C (Green)
         \     /
          \   /
         (Red)
            D
```

---

# 11. Understanding Coloring Rules

Consider

```
A ----- B
```

Rule

```
A = Red

↓

B ≠ Red
```

---

Another example

```
      A
     / \
    /   \
   B-----C
```

If

```
A = Red
```

then

```
B = Blue

C = Green
```

or

```
B = Green

C = Blue
```

Both are valid.

---

Invalid example

```
A = Red

B = Blue

C = Blue
```

Since

```
B and C are adjacent
```

this violates graph coloring.

---

# 12. Chromatic Number

The **Chromatic Number** of a graph is the minimum number of colors required to color the graph without conflicts.

Notation

```
χ(G)
```

Examples

---

## Empty Graph

```
A   B   C
```

Chromatic Number

```
χ(G)=1
```

---

## Path Graph

```
A----B----C----D
```

Chromatic Number

```
χ(G)=2
```

---

## Even Cycle

```
A----B
|    |
D----C
```

Chromatic Number

```
χ(G)=2
```

---

## Odd Cycle

```
     A
   /   \
  B-----C
```

Chromatic Number

```
χ(G)=3
```

---

## Complete Graph

```
K4
```

```
A------B
|\    /|
| \  / |
|  \/  |
|  /\  |
| /  \ |
|/    \|
C------D
```

Chromatic Number

```
χ(G)=4
```

Every vertex needs a unique color.

---

# 13. Types of Graph Coloring Problems

## Vertex Coloring

Most common problem.

Assign colors to vertices.

---

## Edge Coloring

Assign colors to edges.

Adjacent edges cannot share colors.

```
A======B======C
```

---

## Face Coloring

Used in planar graphs.

Each region gets a color.

Commonly used in map coloring.

---

## List Coloring

Each vertex has its own allowed color list.

Example

```
A : {Red, Blue}

B : {Blue}

C : {Green, Red}
```

---

## Total Coloring

Both edges and vertices are colored.

Additional constraints apply.

---

# 14. Key Properties

- Adjacent vertices always have different colors.
- Non-adjacent vertices may have the same color.
- Every graph can be colored using at most **n** colors, where **n** is the number of vertices.
- Finding the minimum number of colors is computationally difficult.
- Graph Coloring belongs to the class of **NP-Complete** problems.
- Backtracking guarantees finding a valid coloring if one exists.
- The algorithm explores the state space systematically.
- Invalid partial colorings are abandoned immediately.

---

# 15. Relationship with NP-Complete Problems

Graph Coloring is one of the classical NP-Complete problems studied in Design and Analysis of Algorithms.

Why?

Suppose there are

- **n vertices**
- **m colors**

Every vertex has **m** possible choices.

The total number of possible assignments is

```
m × m × m × ...

= mⁿ
```

This exponential search space makes exhaustive checking impractical for large graphs.

For example:

| Vertices (n) | Colors (m) | Possible Assignments |
|--------------|------------|----------------------:|
| 4 | 3 | 81 |
| 6 | 3 | 729 |
| 10 | 3 | 59,049 |
| 20 | 3 | 3,486,784,401 |
| 30 | 3 | 205,891,132,094,649 |

Backtracking improves practical performance by **pruning** invalid partial solutions instead of exploring every possible assignment. However, in the worst case, its time complexity remains exponential.

---

## Part 1 Complete

The next part covers:

- Backtracking Algorithm
- State Space Tree
- Recursive Strategy
- Detailed Step-by-Step Dry Run
- Multiple ASCII Diagrams
- Decision Tree
- Backtracking Process
- Pseudocode
- Complete Visual Walkthrough

---

# 16. Backtracking Algorithm

## What is Backtracking?

Backtracking is a recursive problem-solving technique that constructs a solution **incrementally**.

At every step, the algorithm:

1. Chooses one possible option.
2. Checks whether the choice is valid.
3. Continues recursively if valid.
4. If the choice later leads to failure, it **undoes** (backtracks) that choice and tries another option.

For Graph Coloring:

- **Choice:** Assign one of the available colors.
- **Constraint:** Adjacent vertices cannot have the same color.
- **Goal:** Successfully color all vertices.

---

## General Idea

```
Choose a vertex

        │
        ▼

Try Color 1

        │
        ▼

Valid?

 ┌──────┴──────┐
 │             │
Yes            No
 │             │
 ▼             ▼

Color Next   Try Next Color

        │
        ▼

All Vertices Colored?

 ┌──────┴──────┐
 │             │
Yes            No
 │             │
 ▼             ▼

Solution    Backtrack
```

---

# 17. State Space Representation

Every node in the search tree represents a **partial coloring**.

Example:

```
Vertices

A B C D
```

Initially

```
[ _, _, _, _ ]
```

After coloring A

```
[ R, _, _, _ ]
```

After coloring B

```
[ R, G, _, _ ]
```

After coloring C

```
[ R, G, B, _ ]
```

After coloring D

```
[ R, G, B, R ]
```

This becomes a complete solution.

---

# 18. Recursive Strategy

The recursive function colors one vertex at a time.

General strategy

```
Color(Vertex)

Try Color 1

Try Color 2

Try Color 3

...

If valid

    Recur for next vertex

Else

    Try another color

If all colors fail

    Return to previous vertex
```

---

## Recursive Flow

```
Color(A)

↓

Color(B)

↓

Color(C)

↓

Color(D)

↓

Finished
```

If any step fails

```
Color(D)

↓

Failure

↓

Backtrack

↓

Color(C)

↓

Try another color

↓

Continue
```

---

# 19. Safe Coloring Check

Before assigning a color, we must verify that no adjacent vertex already has that color.

Suppose

```
A ----- B
```

Current coloring

```
A = Red
```

Trying

```
B = Red
```

Check

```
Adjacent?

Yes
```

Result

```
Not Safe
```

---

Another example

```
A ----- B
|
|
C
```

Current colors

```
A = Blue

B = Red
```

Trying

```
C = Blue
```

Since C touches A,

```
Not Safe
```

Trying

```
C = Green
```

No conflict.

```
Safe
```

---

# 20. Complete Algorithm Flow

```
Start

   │

Assign first vertex

   │

Try first color

   │

Safe?

 ┌───────────┴───────────┐

 │                       │

No                     Yes

 │                       │

Try Next Color      Assign Color

                         │

                         ▼

              Color Next Vertex

                         │

                All Colored?

                ┌────┴─────┐

                │          │

               Yes        No

                │          │

                ▼          ▼

          Print Solution  Continue

                           │

                     All Colors Fail?

                           │

                   ┌───────┴────────┐

                   │                │

                  No              Yes

                   │                │

                   ▼                ▼

          Try Next Color     Backtrack
```

---

# 21. Example Graph

Consider the graph

```
        A
      /   \
     /     \
    B-------C
     \     /
      \   /
        D
```

Adjacency

```
A : B C

B : A C D

C : A B D

D : B C
```

Available colors

```
Red

Green

Blue
```

---

# 22. Step-by-Step Coloring

## Step 1

Color A

```
A = Red
```

```
        R
        A
      /   \
     /     \
    B-------C
     \     /
      \   /
        D
```

---

## Step 2

Move to B

Try

```
Red
```

Conflict

```
A is already Red
```

Try

```
Green
```

Accepted

```
        R
        A
      /   \
     /     \
   G B------C
      \    /
       \  /
        D
```

---

## Step 3

Move to C

Try

```
Red
```

Conflict with A

Try

```
Green
```

Conflict with B

Try

```
Blue
```

Accepted

```
        R
        A
      /   \
     /     \
   G B------C B
      \    /
       \  /
        D
```

---

## Step 4

Move to D

Try

```
Red
```

Neighbors

```
B = Green

C = Blue
```

No conflict

Assign

```
D = Red
```

Final coloring

```
        R
        A
      /   \
     /     \
   G B------C B
      \    /
       \  /
      R D
```

Solution

```
A = Red

B = Green

C = Blue

D = Red
```

---

# 23. Example Showing Backtracking

Now consider a graph where the first choices fail.

```
        A
      / | \
     /  |  \
    B---C---D
```

Available colors

```
Red

Green
```

---

## Step 1

```
A = Red
```

---

## Step 2

```
B = Green
```

---

## Step 3

Try

```
C = Red
```

Conflict

```
C touches A
```

Try

```
Green
```

Conflict

```
C touches B
```

No color works.

```
Failure
```

---

Algorithm backtracks

```
        A = Red

        ↓

        B = Green

        ↓

        C Failed

        ↓

Backtrack

        ↓

Undo B

        ↓

Try another color
```

---

## Visual Representation

```
A = Red

↓

B = Green

↓

C

↓

Red?

❌

↓

Green?

❌

↓

Backtrack

↓

Undo B

↓

Try another possibility
```

---

# 24. Decision Tree

Example

```
                 Start

                    |

                  A=R

          _________|__________

         /                    \

     B=R(X)               B=G

                             |

                    _________|_________

                   /                   \

              C=R(X)             C=B

                                     |

                             ________|_________

                            /                  \

                       D=R              D=G

                          |                |

                     Solution         Solution
```

Legend

```
R = Red

G = Green

B = Blue

X = Invalid
```

---

# 25. State Space Tree

Each level represents one vertex.

```
Level 0

               Start

Level 1

      R        G        B

Level 2

   RR RG RB

   GR GG GB

   BR BG BB

Level 3

Many more combinations...

Level n

Complete color assignments
```

Backtracking prunes branches such as

```
RR

GG

BB
```

when adjacent vertices become identical.

---

# 26. Recursive Call Stack

Suppose

```
Vertices

A B C D
```

Call stack

```
graphColor(0)

↓

graphColor(1)

↓

graphColor(2)

↓

graphColor(3)

↓

graphColor(4)

↓

Solution
```

If vertex 3 fails

```
graphColor(3)

↓

Return

↓

graphColor(2)

↓

Try another color
```

---

# 27. Pseudocode

```text
GRAPH-COLOR(vertex)

IF vertex == totalVertices

    return TRUE

FOR every color

    IF color is safe

        assign color

        IF GRAPH-COLOR(next vertex)

             return TRUE

        remove color

RETURN FALSE
```

---

# 28. Detailed Pseudocode

```text
GraphColor(vertex)

if vertex == V

      print solution

      return true

for color = 1 to m

      if Safe(vertex,color)

            color[vertex]=color

            if GraphColor(vertex+1)

                    return true

            color[vertex]=0

return false
```

---

# 29. Safe Function Pseudocode

```text
Safe(vertex,color)

for every adjacent vertex

      if adjacent has same color

             return false

return true
```

---

# 30. Why Backtracking Works

The algorithm systematically explores the search space.

It never permanently commits to a color unless the remaining graph can also be colored.

If a future conflict appears,

the previous decision is undone.

Visualization

```
Choose

↓

Validate

↓

Continue

↓

Failure

↓

Undo

↓

Choose Again

↓

Continue
```

This guarantees:

- No valid solution is missed.
- Invalid branches are abandoned immediately.
- The first complete valid assignment found is returned (or all solutions if modified accordingly).

---

# 31. Search Space Pruning

Without pruning:

```
        Root

      / / | \

     / /  |  \

Every branch explored
```

With backtracking:

```
         Root

      /    |    \

     /     |     \

 Invalid  Valid  Invalid

   X        |       X

            |

        Continue
```

Large portions of the search tree are skipped because conflicts are detected early.

---

## Part 2 Complete

The next part includes:

- Complete Java Implementation
- Fully Commented Source Code
- Example Usage
- Dry Run of Java Program
- Time Complexity
- Space Complexity
- Correctness Explanation
- Output Analysis

---

# 32. Java Implementation

The following program implements the **Graph Coloring Problem using Backtracking**.

It uses:

- Adjacency Matrix representation
- Recursive backtracking
- Safe color checking
- `m` available colors
- Prints one valid coloring if it exists

```java
public class GraphColoring {

    // Number of vertices
    private int V;

    // Graph represented using adjacency matrix
    private int[][] graph;

    // Stores assigned color for every vertex
    // 0 means no color assigned
    private int[] color;

    // Constructor
    public GraphColoring(int[][] graph) {
        this.graph = graph;
        this.V = graph.length;
        this.color = new int[V];
    }

    // Check whether assigning color 'c'
    // to vertex 'v' is safe
    private boolean isSafe(int v, int c) {

        for (int i = 0; i < V; i++) {

            // Adjacent vertex with same color
            if (graph[v][i] == 1 && color[i] == c) {
                return false;
            }
        }

        return true;
    }

    // Recursive backtracking function
    private boolean graphColoringUtil(int vertex, int m) {

        // All vertices colored
        if (vertex == V) {
            return true;
        }

        // Try every color
        for (int c = 1; c <= m; c++) {

            if (isSafe(vertex, c)) {

                // Assign color
                color[vertex] = c;

                // Recur for next vertex
                if (graphColoringUtil(vertex + 1, m)) {
                    return true;
                }

                // Backtrack
                color[vertex] = 0;
            }
        }

        return false;
    }

    // Driver function
    public void solve(int m) {

        if (graphColoringUtil(0, m)) {

            System.out.println("Solution Exists\n");

            for (int i = 0; i < V; i++) {
                System.out.println(
                    "Vertex " + i + " --> Color " + color[i]
                );
            }

        } else {

            System.out.println("No Solution Exists");

        }
    }

    public static void main(String[] args) {

        int[][] graph = {

            {0,1,1,0},
            {1,0,1,1},
            {1,1,0,1},
            {0,1,1,0}

        };

        int m = 3;

        GraphColoring obj = new GraphColoring(graph);

        obj.solve(m);
    }
}
```

---

# 33. Code Explanation

## Class Variables

```java
private int V;
```

Stores the total number of vertices.

---

```java
private int[][] graph;
```

Stores the adjacency matrix.

Example

```
      A B C D

A     0 1 1 0

B     1 0 1 1

C     1 1 0 1

D     0 1 1 0
```

---

```java
private int[] color;
```

Stores assigned colors.

Initially

```
Vertex

0 1 2 3

Color

0 0 0 0
```

---

# 34. Understanding isSafe()

Purpose

Checks whether a color can be assigned.

```java
private boolean isSafe(int v, int c)
```

Suppose

```
Trying

Vertex 2

Color 1
```

The function checks every adjacent vertex.

```
for every neighbour

      if neighbour has same color

             return false
```

Otherwise

```
return true
```

---

### Visualization

Current assignment

```
Vertex

A B C D

Color

R G _ R
```

Trying

```
C = Green
```

Neighbors

```
A

B

D
```

Since

```
B = Green
```

Result

```
Not Safe
```

---

# 35. Understanding graphColoringUtil()

This function performs the recursion.

```java
graphColoringUtil(vertex,m)
```

If

```
vertex == V
```

then

```
All vertices colored.
```

Return

```
true
```

Otherwise

```
Try Color 1

Try Color 2

Try Color 3

...
```

Whenever a color works

```
Assign

↓

Recur

↓

If success

Return true
```

Otherwise

```
Undo assignment

Try next color
```

---

# 36. Backtracking in Code

The most important statement is

```java
color[vertex] = 0;
```

This line removes the previously assigned color.

Suppose

```
A = Red

B = Green

C = Blue
```

Later

```
D cannot be colored.
```

Execution

```
Undo C

↓

Try another color

↓

Continue
```

Without removing

```
C = Blue
```

the algorithm would remain stuck.

---

# 37. Example Execution

Input Graph

```
        A
      /   \
     /     \
    B-------C
     \     /
      \   /
        D
```

Colors

```
1

2

3
```

---

### Step 1

```
Vertex A

Try Color 1

Success
```

State

```
A = 1
```

---

### Step 2

```
Vertex B

Try Color 1

Conflict
```

Try

```
Color 2
```

Accepted.

State

```
A = 1

B = 2
```

---

### Step 3

Vertex C

```
Color 1

Conflict
```

```
Color 2

Conflict
```

```
Color 3

Accepted
```

State

```
A = 1

B = 2

C = 3
```

---

### Step 4

Vertex D

```
Color 1

Safe
```

Solution

```
A = 1

B = 2

C = 3

D = 1
```

---

# 38. Dry Run Table

| Step | Vertex | Color Tried | Result |
|------:|---------|------------|---------|
| 1 | A | 1 | Accepted |
| 2 | B | 1 | Rejected |
| 3 | B | 2 | Accepted |
| 4 | C | 1 | Rejected |
| 5 | C | 2 | Rejected |
| 6 | C | 3 | Accepted |
| 7 | D | 1 | Accepted |
| 8 | Finished | — | Solution Found |

---

# 39. Recursive Call Stack

```
graphColor(0)

↓

graphColor(1)

↓

graphColor(2)

↓

graphColor(3)

↓

graphColor(4)

↓

TRUE
```

If failure occurs

```
graphColor(3)

↓

FALSE

↓

Return

↓

graphColor(2)

↓

Try another color
```

---

# 40. Complete Execution Tree

```
                    Start

                      |

                  Vertex 0

             /        |        \

            1         2         3

            |

        Vertex 1

      /     |      \

     1X     2      3

             |

        Vertex 2

    /      |        \

   1X     2X         3

                    |

              Vertex 3

            /     |      \

           1     2X      3X

           |

       Solution
```

Legend

```
X

Invalid assignment
```

---

# 41. Sample Input

Adjacency Matrix

```
0 1 1 0

1 0 1 1

1 1 0 1

0 1 1 0
```

Number of colors

```
3
```

---

# 42. Sample Output

```
Solution Exists

Vertex 0 --> Color 1

Vertex 1 --> Color 2

Vertex 2 --> Color 3

Vertex 3 --> Color 1
```

---

# 43. Time Complexity

Let

```
n

= Number of vertices
```

Each vertex can choose

```
m
```

possible colors.

Worst-case search space

```
m × m × ...

= mⁿ
```

For each assignment,

the algorithm checks all adjacent vertices.

Safety check

```
O(n)
```

Overall worst-case complexity

```
O(n × mⁿ)
```

---

### Example

```
Vertices = 4

Colors = 3
```

Possible assignments

```
3⁴ = 81
```

Vertices

```
10
```

Assignments

```
3¹⁰ = 59049
```

Growth is exponential.

---

# 44. Space Complexity

The algorithm stores

- Color array
- Recursion stack
- Adjacency matrix (input representation)

Auxiliary space due to recursion:

```
O(n)
```

Color array:

```
O(n)
```

If the adjacency matrix is included:

```
Graph Storage = O(n²)
```

Overall:

| Component | Complexity |
|-----------|------------|
| Recursion Stack | O(n) |
| Color Array | O(n) |
| Adjacency Matrix | O(n²) |
| Auxiliary (algorithm only) | O(n) |

---

# 45. Correctness Intuition

The algorithm is correct because:

1. Every assigned color is checked using `isSafe()`.
2. Invalid assignments are never expanded.
3. Every possible valid color choice is eventually explored.
4. Backtracking guarantees that no potential solution is permanently skipped.
5. If a valid coloring exists, the recursive search will eventually reach it.
6. If every combination fails, the algorithm correctly reports that no solution exists.

---

# 46. Advantages of Using Backtracking

- Finds a valid solution if one exists.
- Eliminates invalid branches early.
- Simple recursive implementation.
- Suitable for Constraint Satisfaction Problems (CSPs).
- Can be modified to print all valid colorings instead of only one.
- Easy to understand for academic purposes.

---

## Part 3 Complete

The next part covers:

- Limitations
- Worst-Case Scenarios
- Edge Cases (Empty, Complete, Bipartite, Trees, Cycles)
- Visual Demonstrations
- Comparison with Greedy Coloring
- Exam Tips
- Viva Questions
- Frequently Asked Questions
- Summary and Key Takeaways

---

# 47. Limitations of Backtracking

Although backtracking is guaranteed to find a solution if one exists, it is **not efficient for large graphs** because the search space grows exponentially.

## Major Limitations

- Exponential worst-case running time.
- Not suitable for very large dense graphs.
- May explore many unsuccessful branches.
- Performance depends heavily on graph structure.
- Finding the **minimum** number of colors is computationally difficult.

---

## Visualization

Small Graph

```
A ---- B
 \    /
  \  /
   C
```

Search Tree

```
Root

├── Color 1
├── Color 2
└── Color 3
```

Very few branches.

---

Large Graph

```
20 Vertices

Each vertex

↓

3 possible colors

↓

3²⁰ possibilities

↓

3,486,784,401 assignments
```

Huge search space.

---

# 48. Worst-Case Scenario

The worst case occurs when the algorithm explores almost every possible coloring before determining that no solution exists.

Example

```
      A
     /|\
    / | \
   B--C--D
```

Suppose

```
Available Colors = 2
```

This graph requires **3 colors**.

The algorithm repeatedly tries

```
Color 1

↓

Conflict

↓

Color 2

↓

Conflict

↓

Backtrack

↓

Repeat
```

Almost every branch must be explored.

---

## Worst-Case Search Tree

```
                Root

        /         |         \

      C1         C2         C3

     /|\        /|\        /|\

   ... ...    ... ...    ... ...

        Thousands of branches

               ↓

        No Valid Solution
```

---

# 49. Edge Cases

Understanding edge cases is important for exams and interviews.

---

## Case 1: Empty Graph

No edges.

```
A    B    C    D
```

Only one color is required.

```
A → Red

B → Red

C → Red

D → Red
```

Chromatic Number

```
χ(G)=1
```

---

## Case 2: Single Vertex

```
A
```

Color

```
Red
```

Only one color needed.

---

## Case 3: Complete Graph

Every vertex is connected to every other vertex.

Example

```
K4
```

```
A------B
|\    /|
| \  / |
|  \/  |
|  /\  |
| /  \ |
|/    \|
C------D
```

Required colors

```
4
```

Every vertex needs a unique color.

---

## Case 4: Bipartite Graph

```
A      B
|\    /|
| \  / |
|  \/  |
|  /\  |
| /  \ |
|/    \|
C      D
```

Only two colors.

```
Red

Blue
```

Possible coloring

```
A = Red

C = Red

B = Blue

D = Blue
```

---

## Case 5: Tree

Example

```
        A

      / | \

     B  C  D

       / \

      E   F
```

Every tree can be colored using **2 colors**.

Example

```
A = Red

B = Blue

C = Blue

D = Blue

E = Red

F = Red
```

---

## Case 6: Even Cycle

```
A----B

|    |

D----C
```

Only

```
2 colors
```

Possible coloring

```
A = Red

B = Blue

C = Red

D = Blue
```

---

## Case 7: Odd Cycle

Triangle

```
     A

   /   \

  B-----C
```

Requires

```
3 colors
```

---

## Case 8: Disconnected Graph

```
A----B


C----D
```

Color each connected component independently.

---

# 50. When Backtracking Performs Well

Backtracking performs efficiently when:

- Few vertices.
- Sparse graph.
- Many colors available.
- Conflicts detected early.
- Invalid branches are pruned quickly.

Example

```
A----B

|

C
```

Three colors

```
Red

Green

Blue
```

Almost no backtracking occurs.

---

# 51. When Backtracking Performs Poorly

Performance decreases when:

- Dense graph.
- Many edges.
- Very few colors.
- Large number of vertices.
- Solution does not exist.

Visualization

```
Complete Graph

K8
```

```
Every vertex

↓

Connected to all others

↓

Frequent conflicts

↓

Heavy backtracking
```

---

# 52. Comparison with Greedy Coloring

| Feature | Backtracking | Greedy Coloring |
|----------|--------------|-----------------|
| Always Finds Solution (if exists) | Yes | Not Always |
| Guarantees Correctness | Yes | Yes (valid coloring, but not minimum colors) |
| Finds Minimum Colors Automatically | No (unless solving the optimization version) | No |
| Worst-Case Complexity | Exponential | Polynomial |
| Suitable for Large Graphs | No | Yes |
| Uses Recursion | Yes | Usually No |
| Prunes Invalid Choices | Yes | No |

---

## Example

Graph

```
A-----B

|

C
```

Greedy

```
A = Red

B = Blue

C = Blue
```

Backtracking

```
Explores

↓

Checks

↓

Backtracks if needed

↓

Produces a valid assignment
```

---

# 53. Advantages

- Systematic exploration of solutions.
- Guarantees finding a valid coloring if one exists.
- Simple recursive implementation.
- Easy to understand.
- Useful for Constraint Satisfaction Problems.
- Demonstrates recursion and state-space search clearly.
- Can be extended to enumerate all solutions.

---

# 54. Disadvantages

- Exponential running time.
- Not practical for very large graphs.
- High recursive overhead.
- May revisit many partial assignments.
- Poor scalability for dense graphs.

---

# 55. Common Mistakes

### Mistake 1

Not checking adjacency before assigning a color.

Incorrect

```
A = Red

B = Red
```

---

### Mistake 2

Forgetting to remove the color during backtracking.

Incorrect

```java
color[v] = c;

// Missing

color[v] = 0;
```

This prevents the algorithm from exploring alternative assignments.

---

### Mistake 3

Stopping recursion too early.

Correct base case

```java
if(vertex == V)
    return true;
```

---

### Mistake 4

Using an incorrect adjacency matrix.

Matrix must be symmetric for an undirected graph.

Correct

```
0 1 1

1 0 0

1 0 0
```

---

# 56. Frequently Asked Viva Questions

### Q1. What is Graph Coloring?

Assigning colors to vertices so that adjacent vertices have different colors.

---

### Q2. Which algorithmic paradigm is used?

Backtracking.

---

### Q3. Why is graph coloring difficult?

Finding the minimum number of colors is an NP-Complete problem.

---

### Q4. What is the Chromatic Number?

The minimum number of colors required to color a graph.

---

### Q5. Why is `isSafe()` required?

It verifies that assigning a color does not violate the adjacency constraint.

---

### Q6. Why do we backtrack?

To undo an incorrect choice and explore another possibility.

---

### Q7. What is the worst-case time complexity?

```
O(n × mⁿ)
```

where:

- `n` = number of vertices
- `m` = number of available colors

---

### Q8. What is the auxiliary space complexity?

```
O(n)
```

due to the recursion stack and color array.

---

### Q9. Can multiple solutions exist?

Yes. A graph may have many valid color assignments.

---

### Q10. Does this algorithm always find the minimum number of colors?

No. It determines whether the graph can be colored using the given value of `m`. To find the chromatic number, the algorithm can be executed repeatedly with increasing values of `m`, starting from 1.

---

# 57. Exam Tips

Remember the following sequence:

```
Choose

↓

Check

↓

Assign

↓

Recur

↓

Fail?

↓

Backtrack

↓

Try Next Color
```

This is the core workflow of the algorithm.

---

### Important Formula

Worst-case time complexity

```
O(n × mⁿ)
```

Auxiliary space complexity

```
O(n)
```

---

### Points Commonly Asked in Exams

- Definition of Graph Coloring.
- Chromatic Number.
- State-space tree.
- Backtracking process.
- `isSafe()` function.
- Recursive algorithm.
- Dry run.
- Java implementation.
- Time and space complexity.
- Applications.

---

# 58. Summary

Graph Coloring is a classic **Constraint Satisfaction Problem (CSP)** in which adjacent vertices must receive different colors.

Backtracking solves the problem by:

1. Coloring one vertex at a time.
2. Verifying whether the assignment is safe.
3. Recursively coloring the remaining vertices.
4. Undoing assignments that lead to dead ends.
5. Continuing until a valid coloring is found or all possibilities are exhausted.

The algorithm is **complete** (it will find a solution if one exists) but has **exponential worst-case complexity**, making it suitable primarily for small to medium-sized graphs or educational purposes.

---

# 59. Key Takeaways

- Graph Coloring assigns colors to vertices while ensuring adjacent vertices have different colors.
- The minimum number of required colors is called the **Chromatic Number**.
- Backtracking explores color assignments recursively.
- The `isSafe()` function prevents invalid assignments.
- Backtracking removes incorrect assignments and tries alternatives.
- Worst-case time complexity is **O(n × mⁿ)**.
- Auxiliary space complexity is **O(n)**.
- Common applications include timetable scheduling, register allocation, frequency assignment, map coloring, and task scheduling.
- Graph Coloring is a fundamental NP-Complete problem in Design and Analysis of Algorithms.

---

# 60. One-Page Revision Sheet

## Workflow

```text
Start
   │
Choose Vertex
   │
Try a Color
   │
Safe?
 ├── No → Try Next Color
 └── Yes
        │
 Assign Color
        │
 Next Vertex
        │
 All Vertices Colored?
 ├── Yes → Solution
 └── No
        │
 Failure?
 ├── No → Continue
 └── Yes
        │
 Backtrack
        │
 Remove Color
        │
 Try Another Color
```

---

## Complexity Cheat Sheet

| Property | Value |
|----------|-------|
| Paradigm | Backtracking |
| Data Structure | Graph (Adjacency Matrix/List) |
| Decision | Choose one of `m` colors |
| Constraint | Adjacent vertices must differ |
| Worst-Case Time | **O(n × mⁿ)** |
| Auxiliary Space | **O(n)** |
| Graph Storage (Adjacency Matrix) | **O(n²)** |
| Problem Class | NP-Complete |

---

## Applications

- University Timetabling
- Register Allocation
- Map Coloring
- Wireless Channel Assignment
- Frequency Allocation
- Task Scheduling
- PCB Design
- Network Resource Allocation

---

**End of Document**


