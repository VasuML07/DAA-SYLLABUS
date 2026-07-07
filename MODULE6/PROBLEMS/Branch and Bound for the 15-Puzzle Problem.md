# Branch and Bound for the 15-Puzzle Problem

> **Subject:** Design and Analysis of Algorithms (DAA)  
> **Topic:** Branch and Bound Applied to the 15-Puzzle Problem  
> **Purpose:** Engineering Semester Exam Study Notes

---

# Table of Contents

- [1. Introduction](#1-introduction)
- [2. What is Branch and Bound?](#2-what-is-branch-and-bound)
- [3. Why Branch and Bound?](#3-why-branch-and-bound)
- [4. When to Use Branch and Bound](#4-when-to-use-branch-and-bound)
- [5. Where Branch and Bound is Applied](#5-where-branch-and-bound-is-applied)
- [6. Basic Terminology](#6-basic-terminology)
- [7. Branch and Bound Working Principle](#7-branch-and-bound-working-principle)
- [8. Generic Branch and Bound Algorithm](#8-generic-branch-and-bound-algorithm)
- [9. Understanding the 15-Puzzle Problem](#9-understanding-the-15-puzzle-problem)
- [10. Puzzle Representation](#10-puzzle-representation)
- [11. Legal Moves in the Puzzle](#11-legal-moves-in-the-puzzle)
- [12. State Space of the Puzzle](#12-state-space-of-the-puzzle)
- [13. Goal State](#13-goal-state)
- [14. Solvability of the 15-Puzzle](#14-solvability-of-the-15-puzzle)
- [15. Why Branch and Bound Works Well for the 15-Puzzle](#15-why-branch-and-bound-works-well-for-the-15-puzzle)

---

# 1. Introduction

The **15-Puzzle** is one of the most famous search problems in Artificial Intelligence and Algorithm Design.

It consists of **15 numbered tiles** and **one empty space** arranged inside a **4 × 4 grid**.

The objective is to rearrange the tiles into the correct order by sliding one tile at a time into the empty space.

Unlike simple sorting problems, the 15-puzzle has an enormous search space.

Even modern computers cannot examine every possible arrangement one by one.

This is where **Branch and Bound** becomes useful.

Instead of blindly exploring every possible move, it intelligently selects promising states and ignores states that cannot produce a better solution.

---

# 2. What is Branch and Bound?

**Branch and Bound** is an optimization and state-space search technique.

It systematically explores possible solutions while using a **bound (cost estimate)** to eliminate unnecessary searches.

Instead of checking every possibility:

- Generate possible states (**Branch**)
- Estimate their quality (**Bound**)
- Explore only the most promising state first
- Ignore states that cannot improve the current best solution

---

## Basic Idea

```
Generate States
       │
       ▼
Estimate Cost
       │
       ▼
Choose Best State
       │
       ▼
Expand It
       │
       ▼
Repeat Until Goal
```

---

# 3. Why Branch and Bound?

Many algorithmic problems have extremely large search spaces.

For example,

```
15-Puzzle

Possible arrangements

≈ 20,922,789,888,000
(Over 20 trillion states)
```

Trying every state is impossible.

Branch and Bound solves this issue by:

- Exploring only useful paths
- Ignoring poor choices
- Using heuristic estimates
- Finding an optimal solution

---

## Motivation

Suppose you are in a maze.

Two approaches exist.

### Method 1

Explore every road.

```
Road A

Road B

Road C

Road D

Road E

...
```

Very slow.

---

### Method 2

Estimate which road appears closest to the destination.

Explore that first.

```
Road A      Cost = 20

Road B      Cost = 12

Road C      Cost = 35

Road D      Cost = 8   ← Choose

Road E      Cost = 30
```

This is exactly how Branch and Bound works.

---

# 4. When to Use Branch and Bound

Branch and Bound is useful when:

- Solution space is extremely large
- Need an optimal solution
- Brute force is too expensive
- A good lower-bound estimate exists
- Many branches can be discarded

---

## Typical Characteristics

✔ Large search space

✔ Optimization problem

✔ Need shortest path

✔ Need minimum cost

✔ State-space representation

✔ Good heuristic available

---

# 5. Where Branch and Bound is Applied

Branch and Bound is used in many optimization problems.

| Problem | Goal |
|----------|------|
| 15 Puzzle | Minimum moves |
| 8 Puzzle | Minimum moves |
| Traveling Salesman | Minimum tour cost |
| Assignment Problem | Minimum assignment cost |
| Job Scheduling | Minimum completion time |
| Knapsack Problem | Maximum profit |
| Robot Navigation | Shortest path |
| AI Game Search | Best move |

---

## Real-World Applications

### GPS Navigation

```
Current Location
        │
        ▼
Estimate Remaining Distance
        │
        ▼
Choose Best Road
```

---

### Robotics

Robots search for the shortest movement sequence while avoiding unnecessary exploration.

---

### Artificial Intelligence

Puzzle solving

Game playing

State-space planning

---

### Logistics

Finding optimal delivery routes.

---

### Manufacturing

Machine scheduling.

---

### Network Routing

Finding minimum communication cost.

---

# 6. Basic Terminology

Understanding Branch and Bound requires knowing several important terms.

---

## State

A single arrangement of the puzzle.

Example

```
1  2  3  4
5  6  7  8
9 10 11 12
13 14  0 15
```

Here

`0` represents the blank tile.

---

## Initial State

Starting arrangement.

Example

```
1  2  3  4
5  6  7  8
9 10 11 12
13  0 14 15
```

---

## Goal State

Desired arrangement.

```
1  2  3  4
5  6  7  8
9 10 11 12
13 14 15  0
```

---

## Node

A node contains

- Current state
- Parent
- Cost
- Level
- Bound

---

## Branch

Generating child nodes.

Example

```
Current State

        │
        ▼

Move Left

Move Right

Move Up

Move Down
```

Each move creates a new branch.

---

## Bound

An estimate of how promising a state is.

Smaller bound

↓

Better state

---

## Live Node

Generated but not yet expanded.

---

## Dead Node

Already explored.

---

## Optimal Solution

Shortest sequence of moves from start to goal.

---

# 7. Branch and Bound Working Principle

The algorithm repeatedly performs four operations.

```
Current Node
      │
      ▼
Generate Children
      │
      ▼
Calculate Bound
      │
      ▼
Insert into Priority Queue
      │
      ▼
Choose Lowest Bound
      │
      ▼
Repeat
```

---

## Detailed Workflow

```
START

   │

Generate Root

   │

Compute Bound

   │

Insert into Priority Queue

   │

Take Best Node

   │

Goal?

 ┌───────┐
 │ Yes   │────► STOP
 └───────┘

     │ No

Generate Children

Compute Bound

Insert Again

Repeat
```

---

# 8. Generic Branch and Bound Algorithm

```
Create root node

Compute its bound

Insert root into priority queue

while queue is not empty

    remove node with minimum bound

    if goal reached

         return solution

    generate child states

    compute bound

    insert promising states

end while
```

---

## Generic Flowchart

```
                 START
                    │
                    ▼
          Create Initial Node
                    │
                    ▼
           Compute Bound
                    │
                    ▼
       Insert into Priority Queue
                    │
                    ▼
      Remove Best Available Node
                    │
         ┌──────────┴──────────┐
         │                     │
         ▼                     ▼
     Goal Found?             No
         │                     │
        Yes                    ▼
         │            Generate Children
         │                     │
         ▼                     ▼
   Return Solution      Compute Bounds
                               │
                               ▼
                    Insert into Queue
                               │
                               ▼
                           Repeat
```

---

# 9. Understanding the 15-Puzzle Problem

The puzzle consists of

- 15 numbered tiles
- One empty position
- A 4 × 4 board

---

## Initial Example

```
+----+----+----+----+
| 1  | 2  | 3  | 4  |
+----+----+----+----+
| 5  | 6  | 7  | 8  |
+----+----+----+----+
| 9  |10  |11  |12  |
+----+----+----+----+
|13  |14  | 0  |15  |
+----+----+----+----+
```

---

## Goal

```
+----+----+----+----+
| 1  | 2  | 3  | 4  |
+----+----+----+----+
| 5  | 6  | 7  | 8  |
+----+----+----+----+
| 9  |10  |11  |12  |
+----+----+----+----+
|13  |14  |15  | 0  |
+----+----+----+----+
```

---

# 10. Puzzle Representation

The board is represented using a **4 × 4 matrix**.

```
int[][] board =
{
 {1,2,3,4},
 {5,6,7,8},
 {9,10,11,12},
 {13,14,0,15}
};
```

Each position stores

- Tile number
- Blank tile (0)

---

## Matrix Layout

```
(0,0) (0,1) (0,2) (0,3)

(1,0) (1,1) (1,2) (1,3)

(2,0) (2,1) (2,2) (2,3)

(3,0) (3,1) (3,2) (3,3)
```

---

# 11. Legal Moves in the Puzzle

Only the blank tile moves.

Possible directions

```
        UP
         ↑

LEFT ← Blank → RIGHT

         ↓
       DOWN
```

Example

Current

```
13 14  0 15
```

Possible moves

```
Left

13  0 14 15

-----------------

Right

13 14 15 0

-----------------

Up

13 14 11 15
```

Some moves become illegal near edges.

---

# 12. State Space of the Puzzle

Every valid arrangement is called a **state**.

```
Initial

        │

 ┌──────┼──────┐

 ▼      ▼      ▼

Move   Move   Move

Left    Up    Right

        │

Generate More States
```

The complete search tree contains millions of reachable states.

Branch and Bound explores only the most promising ones.

---

# 13. Goal State

The algorithm stops when

```
1  2  3  4
5  6  7  8
9 10 11 12
13 14 15  0
```

is reached.

At this point,

- Solution path is reconstructed.
- Minimum number of moves is obtained.
- Search terminates.

---

# 14. Solvability of the 15-Puzzle

Not every arrangement can be solved.

Before running the algorithm, we should check whether the puzzle is **solvable**.

---

## Inversions

An **inversion** occurs when a larger numbered tile appears before a smaller numbered tile.

Example

```
1 2 4 3
```

Here

```
4 comes before 3
```

So,

```
Inversions = 1
```

---

## Solvability Rule (4 × 4 Puzzle)

A puzzle is solvable based on:

- Number of inversions
- Row position of the blank tile (counted from the bottom)

Both conditions together determine whether the puzzle has a solution.

Checking solvability first avoids wasting time searching an impossible puzzle.

---

# 15. Why Branch and Bound Works Well for the 15-Puzzle

Without intelligent search,

```
Millions

↓

Billions

↓

Trillions

of states
```

would have to be explored.

Branch and Bound significantly reduces the search effort by prioritizing states with the **lowest estimated total cost**.

Typical cost function:

```
f(n) = g(n) + h(n)
```

Where:

- **g(n)** = Number of moves made from the initial state.
- **h(n)** = Estimated moves remaining (commonly Manhattan Distance).
- **f(n)** = Estimated total cost.

Example:

| State | g(n) | h(n) | f(n) |
|-------|------|------|------|
| A | 2 | 6 | 8 |
| B | 3 | 2 | 5 |
| C | 1 | 9 | 10 |

The algorithm expands **State B** first because it has the smallest estimated cost (`f(n) = 5`).

---

# Key Takeaways

- **Branch and Bound** is an informed state-space search technique.
- It combines **branching** with **cost estimation (bounds)** to reduce unnecessary exploration.
- The **15-puzzle** is a classic optimization/search problem with an enormous state space.
- Each puzzle configuration is treated as a **state**, and each legal tile movement creates a **branch**.
- A **priority queue** is used to always expand the most promising state.
- The common evaluation function is **`f(n) = g(n) + h(n)`**, where `h(n)` is often the Manhattan Distance.
- Checking **solvability** before searching prevents wasted computation on impossible puzzle configurations.

---

**End of Part 1**

**Next Part:** *Algorithm Walkthrough, Manhattan Distance Heuristic, Branch-and-Bound Search Tree, Priority Queue Operations, Detailed Worked Example, Complexity Analysis, and ASCII Flowcharts.*

# 16. Branch and Bound Algorithm for the 15-Puzzle

Unlike brute-force search, **Branch and Bound** expands only the most promising puzzle states. Every generated state is evaluated using a **cost function (bound)**, and the state with the **lowest estimated total cost** is explored first.

---

## Basic Idea

Every puzzle state stores two costs:

- **Actual Cost (`g(n)`)** → Number of moves performed from the initial state.
- **Heuristic Cost (`h(n)`)** → Estimated number of moves remaining to reach the goal.
- **Total Cost (`f(n)`)** → Sum of the above.

```text
f(n) = g(n) + h(n)
```

The algorithm always expands the state with the **minimum `f(n)`**.

---

# 17. State Representation

Each node contains all information required to continue the search.

```text
+----------------------+
| Current Puzzle State |
+----------------------+
| Parent Node          |
| Level (g)            |
| Heuristic (h)        |
| Total Cost (f)       |
+----------------------+
```

---

## Example Node

```text
State

1  2  3  4
5  6  7  8
9 10 11 12
13  0 14 15

g = 2

h = 2

f = 4
```

---

# 18. Manhattan Distance Heuristic

The most commonly used **bound function** for the 15-puzzle is the **Manhattan Distance**.

It measures how far every tile is from its goal position.

Distance is computed using

```text
|Current Row − Goal Row|
+
|Current Column − Goal Column|
```

---

## Example

Current

```text
1  2  3  4
5  6  7  8
9 10 11 12
13  0 14 15
```

Goal

```text
1  2  3  4
5  6  7  8
9 10 11 12
13 14 15  0
```

---

### Tile 14

Current Position

```
(3,2)
```

Goal Position

```
(3,1)
```

Distance

```
|3-3| + |2-1|

=0+1

=1
```

---

### Tile 15

Current Position

```
(3,3)
```

Goal Position

```
(3,2)
```

Distance

```
|3-3|+|3-2|

=1
```

---

### Total Manhattan Distance

```
Tile 14 = 1

Tile 15 = 1

----------------

Total = 2
```

Therefore,

```
h(n)=2
```

---

# Why Manhattan Distance?

Advantages

- Very fast
- Never overestimates
- Produces optimal solution
- Better than Misplaced Tiles heuristic
- Guides search efficiently

---

# 19. Cost Function

Every node stores

```text
g(n)

+

h(n)

=

f(n)
```

Example

```
Moves Already Taken

g = 5

Estimated Remaining

h = 8

Total

f = 13
```

Priority Queue always selects

```
Lowest f(n)
```

---

# 20. Priority Queue

Instead of using a normal queue, Branch and Bound uses a **Priority Queue (Min Heap)**.

States with the lowest cost leave the queue first.

---

## Illustration

```
Priority Queue

+----------------+

Node A

f=11

+----------------+

Node B

f=5

+----------------+

Node C

f=8

+----------------+
```

Removal Order

```
First

Node B

↓

Second

Node C

↓

Third

Node A
```

---

# 21. Search Tree

Suppose the blank tile can move in three directions.

```
                    Root

                      │

      ┌───────────────┼───────────────┐

      ▼               ▼               ▼

   Left             Right            Up

     │                │               │

 ┌───┴───┐       ┌────┴────┐      ┌───┴───┐

 ▼       ▼       ▼         ▼      ▼       ▼

L1      L2      R1        R2     U1      U2
```

Each child gets

```
New Puzzle

+

New Cost

+

New Manhattan Distance
```

---

# 22. Branching Example

Initial State

```text
1 2 3 4

5 6 7 8

9 10 11 12

13 0 14 15
```

Blank is located here

```
↓

13 0 14 15
```

Possible Moves

```
Move Left

0 13 14 15

--------------------

Move Right

13 14 0 15

--------------------

Move Up

13 10 14 15
```

Each move creates one child node.

---

# 23. Bound Calculation Example

Suppose three children are generated.

| Child | g(n) | h(n) | f(n) |
|-------|------|------|------|
| Left | 1 | 8 | 9 |
| Right | 1 | 2 | 3 |
| Up | 1 | 6 | 7 |

Priority Queue

```
Right

↓

Up

↓

Left
```

Reason

```
Lowest Cost

↓

Best Candidate
```

---

# 24. Complete Algorithm Flow

```text
                 START
                    │
                    ▼
          Read Initial Puzzle
                    │
                    ▼
        Compute Manhattan Distance
                    │
                    ▼
      Insert Initial State into Queue
                    │
                    ▼
      Remove Lowest Cost Node
                    │
          ┌─────────┴─────────┐
          │                   │
          ▼                   ▼
     Goal State?             No
          │                   │
         Yes                  ▼
          │           Generate Children
          │                   │
          ▼                   ▼
     Print Solution    Compute Bound
                              │
                              ▼
                 Insert into Priority Queue
                              │
                              ▼
                          Repeat
```

---

# 25. Step-by-Step Execution

Initial Puzzle

```text
1 2 3 4

5 6 7 8

9 10 11 12

13 0 14 15
```

Suppose

```
g=0

h=2

f=2
```

Queue

```
Root
```

---

## Step 1

Expand Root

Generate

```
Left

Right

Up
```

Calculate Costs

| Move | g | h | f |
|------|---|---|---|
| Left |1|4|5|
| Right|1|0|1|
| Up|1|3|4|

Queue

```
Right

↓

Up

↓

Left
```

---

## Step 2

Remove

```
Right
```

Puzzle

```text
1 2 3 4

5 6 7 8

9 10 11 12

13 14 15 0
```

Goal reached.

Search stops.

---

# 26. Search Tree of the Example

```text
                     Root

                 f=2

                   │

     ┌─────────────┼─────────────┐

     ▼             ▼             ▼

 Left           Right          Up

 f=5             f=1           f=4

                 │

                 ▼

              Goal State
```

---

# 27. Pseudocode

```text
Create Root

Compute Manhattan Distance

Insert Root into Priority Queue

while Queue is not Empty

      Remove Minimum Cost Node

      if Goal Found

            Return Solution

      Generate Valid Children

      Compute Cost

      Insert into Queue

end while
```

---

# 28. Time Complexity

Worst Case

```text
O(b^d)
```

where

- **b** = Branching factor
- **d** = Solution depth

Since the 15-puzzle has a huge search space, the worst-case complexity remains exponential.

---

# 29. Space Complexity

Every generated node may remain inside the priority queue.

```
Space

O(b^d)
```

Memory usage is one of the biggest limitations of Branch and Bound.

---

# 30. Important Exam Points

- Uses **state-space search**.
- Expands the **lowest-cost node first**.
- Maintains a **priority queue (min-heap)**.
- Uses **Manhattan Distance** as the heuristic (bound).
- Guarantees an **optimal solution** when the heuristic is admissible.
- Avoids exploring many unnecessary branches compared to brute-force search.

---

**End of Part 2**

**Next Part:** Complete production-quality Java implementation (`State`, `Node`, `PuzzleSolver`), detailed code explanation, path reconstruction, sample input/output, limitations, edge cases, comparison with BFS/DFS/A*, viva questions, and exam cheat sheet.


# 31. Java Implementation (Part 3A)

This section implements the **core classes** required for solving the **15-Puzzle using Branch and Bound**.

The implementation consists of:

- `Node` class
- Puzzle representation
- Manhattan Distance heuristic
- Utility methods
- Child generation
- Priority Queue setup

> **Note:** The `solve()` algorithm and `main()` method are covered in **Part 3B**.

---

# Project Structure

```text
BranchAndBound15Puzzle
│
├── PuzzleSolver.java
├── Node.java
└── Main.java
```

For simplicity, all classes may also be placed inside a single Java file.

---

# Required Imports

```java
import java.util.*;
```

---

# Node Class

Each node represents one state of the puzzle.

A node stores

- Current board
- Parent node
- Blank tile position
- Level (depth)
- Cost (Manhattan Distance)

---

```java
class Node {

    int[][] board;

    int x;
    int y;

    int cost;

    int level;

    Node parent;

    Node(int[][] board,
         int x,
         int y,
         int newX,
         int newY,
         int level,
         Node parent) {

        this.board = new int[4][4];

        for (int i = 0; i < 4; i++) {
            this.board[i] = board[i].clone();
        }

        int temp = this.board[x][y];
        this.board[x][y] = this.board[newX][newY];
        this.board[newX][newY] = temp;

        this.x = newX;
        this.y = newY;

        this.parent = parent;

        this.level = level;
    }
}
```

---

# Explanation

```text
board
↓

Stores current puzzle

-----------------------

x,y

↓

Current blank position

-----------------------

cost

↓

Heuristic value

-----------------------

level

↓

Number of moves made

-----------------------

parent

↓

Used to reconstruct solution path
```

---

# Goal Configuration

The solver compares every generated state with the goal board.

```java
static final int[][] GOAL = {
        {1,2,3,4},
        {5,6,7,8},
        {9,10,11,12},
        {13,14,15,0}
};
```

---

# Board Size

```java
static final int N = 4;
```

---

# Direction Arrays

Blank tile can move

```text
UP

DOWN

LEFT

RIGHT
```

Direction vectors

```java
static final int[] row = {-1,1,0,0};

static final int[] col = {0,0,-1,1};
```

Meaning

```text
(-1,0)

↓

Move Up

-----------------

(1,0)

↓

Move Down

-----------------

(0,-1)

↓

Move Left

-----------------

(0,1)

↓

Move Right
```

---

# Check Valid Move

```java
static boolean isSafe(int x, int y) {

    return (x >= 0 &&
            x < N &&
            y >= 0 &&
            y < N);

}
```

Example

```text
Board

0 ≤ row < 4

0 ≤ column < 4
```

Invalid

```text
(-1,2)

(4,1)

(2,-1)

(1,4)
```

---

# Print Puzzle

```java
static void printBoard(int[][] board) {

    for(int i=0;i<N;i++) {

        for(int j=0;j<N;j++) {

            System.out.printf("%2d ", board[i][j]);

        }

        System.out.println();

    }

}
```

Output

```text
 1  2  3  4
 5  6  7  8
 9 10 11 12
13 14  0 15
```

---

# Manhattan Distance Function

This is the **bound function**.

```java
static int calculateCost(int[][] board) {

    int cost = 0;

    for(int i=0;i<N;i++) {

        for(int j=0;j<N;j++) {

            if(board[i][j] != 0) {

                int value = board[i][j];

                int goalRow = (value - 1) / N;

                int goalCol = (value - 1) % N;

                cost += Math.abs(i - goalRow)
                        + Math.abs(j - goalCol);

            }

        }

    }

    return cost;

}
```

---

# How Manhattan Distance Works

Current

```text
13 0 14 15
```

Goal

```text
13 14 15 0
```

Tile 14

```text
Current

(3,2)

Goal

(3,1)

Distance

1
```

Tile 15

```text
Current

(3,3)

Goal

(3,2)

Distance

1
```

Total

```text
1+1=2
```

---

# Calculate Total Cost

Branch and Bound uses

```text
f(n)

=

g(n)

+

h(n)
```

Helper method

```java
static int totalCost(Node node) {

    return node.level + node.cost;

}
```

Example

```text
Moves

3

Heuristic

6

Total

9
```

---

# Priority Queue Comparator

The queue always removes the node having the **lowest total cost**.

```java
PriorityQueue<Node> priorityQueue =
        new PriorityQueue<>(
                Comparator.comparingInt(
                        node -> node.level + node.cost
                )
        );
```

Visualization

```text
Priority Queue

-------------------

Node

f=4

-------------------

Node

f=7

-------------------

Node

f=2

-------------------
```

Removal

```text
First

↓

f=2

↓

f=4

↓

f=7
```

---

# Create Child Node

```java
static Node createChild(Node parent,
                        int newX,
                        int newY) {

    Node child = new Node(
            parent.board,
            parent.x,
            parent.y,
            newX,
            newY,
            parent.level + 1,
            parent
    );

    child.cost = calculateCost(child.board);

    return child;

}
```

---

# Child Generation

Suppose blank tile

```text
13 0 14 15
```

Possible children

```text
LEFT

0 13 14 15

------------------

RIGHT

13 14 0 15

------------------

UP

13 10 14 15
```

Each child

- Has new board
- New level
- New heuristic
- Parent pointer

---

# Recursive Path Printing

After reaching the goal, we print every move from the initial state.

```java
static void printPath(Node root) {

    if(root == null)
        return;

    printPath(root.parent);

    printBoard(root.board);

    System.out.println();

}
```

Example Output

```text
Initial

↓

Move 1

↓

Move 2

↓

Move 3

↓

Goal
```

---

# Avoid Revisiting States

To prevent infinite loops, maintain a set of visited configurations.

```java
HashSet<String> visited = new HashSet<>();
```

Convert board into a unique string.

```java
static String boardToString(int[][] board) {

    StringBuilder sb = new StringBuilder();

    for(int[] row : board) {

        for(int value : row) {

            sb.append(value).append(",");

        }

    }

    return sb.toString();

}
```

Example

```text
1,2,3,4,
5,6,7,8,
9,10,11,12,
13,14,0,15,
```

Before adding a child

```java
String key = boardToString(child.board);

if(!visited.contains(key)) {

    visited.add(key);

    priorityQueue.add(child);

}
```

---

# Overall Workflow So Far

```text
Read Initial Puzzle
        │
        ▼
Create Root Node
        │
        ▼
Calculate Manhattan Distance
        │
        ▼
Insert into Priority Queue
        │
        ▼
Remove Best Node
        │
        ▼
Generate Children
        │
        ▼
Calculate Cost
        │
        ▼
Avoid Duplicate States
        │
        ▼
Insert Back into Queue
```

---

# Summary

In this part, we implemented:

- `Node` class
- Puzzle board representation
- Goal configuration
- Direction vectors
- Safe move checking
- Board printing
- Manhattan Distance heuristic
- Total cost calculation (`f = g + h`)
- Priority Queue ordering
- Child node creation
- Path reconstruction
- Duplicate state detection

---

**End of Part 3A**

**Next (Part 3B):** Complete `solve()` method, Branch and Bound search loop, goal detection, sample execution, full `Main` class, input/output examples, and complete runnable Java program.

# 32. Java Implementation (Part 3B)

In **Part 3A**, we implemented:

- `Node` class
- Manhattan Distance heuristic
- Priority Queue
- Child generation
- Utility methods

In this section, we complete the **Branch and Bound algorithm** by implementing the search procedure, goal detection, and the `main()` method.

---

# Complete Solver Class

```java
import java.util.*;

public class PuzzleSolver {

    static final int N = 4;

    static final int[] row = {-1, 1, 0, 0};
    static final int[] col = {0, 0, -1, 1};

    static final int[][] GOAL = {
            {1,2,3,4},
            {5,6,7,8},
            {9,10,11,12},
            {13,14,15,0}
    };

    static class Node {

        int[][] board;

        int x, y;

        int cost;

        int level;

        Node parent;

        Node(int[][] board,
             int x,
             int y,
             int newX,
             int newY,
             int level,
             Node parent) {

            this.board = new int[N][N];

            for(int i=0;i<N;i++)
                this.board[i]=board[i].clone();

            int temp=this.board[x][y];
            this.board[x][y]=this.board[newX][newY];
            this.board[newX][newY]=temp;

            this.x=newX;
            this.y=newY;

            this.level=level;

            this.parent=parent;
        }
    }

    static boolean isSafe(int x,int y){

        return x>=0 && x<N && y>=0 && y<N;

    }

    static int calculateCost(int[][] board){

        int cost=0;

        for(int i=0;i<N;i++){

            for(int j=0;j<N;j++){

                if(board[i][j]!=0){

                    int value=board[i][j];

                    int goalRow=(value-1)/N;

                    int goalCol=(value-1)%N;

                    cost+=Math.abs(i-goalRow)+Math.abs(j-goalCol);

                }

            }

        }

        return cost;

    }

    static void printBoard(int[][] board){

        for(int i=0;i<N;i++){

            for(int j=0;j<N;j++){

                System.out.printf("%2d ",board[i][j]);

            }

            System.out.println();

        }

    }

    static void printPath(Node node){

        if(node==null)
            return;

        printPath(node.parent);

        printBoard(node.board);

        System.out.println();

    }

    static String boardToString(int[][] board){

        StringBuilder sb=new StringBuilder();

        for(int[] r:board){

            for(int value:r){

                sb.append(value).append(",");

            }

        }

        return sb.toString();

    }

    static Node createChild(Node parent,int newX,int newY){

        Node child=new Node(
                parent.board,
                parent.x,
                parent.y,
                newX,
                newY,
                parent.level+1,
                parent
        );

        child.cost=calculateCost(child.board);

        return child;

    }

    static void solve(int[][] initial,int blankX,int blankY){

        PriorityQueue<Node> pq=
                new PriorityQueue<>(
                        Comparator.comparingInt(
                                node->node.cost+node.level
                        )
                );

        HashSet<String> visited=new HashSet<>();

        Node root=new Node(
                initial,
                blankX,
                blankY,
                blankX,
                blankY,
                0,
                null
        );

        root.cost=calculateCost(root.board);

        pq.add(root);

        visited.add(boardToString(root.board));

        while(!pq.isEmpty()){

            Node current=pq.poll();

            if(current.cost==0){

                System.out.println("Goal State Reached");

                System.out.println();

                System.out.println("Minimum Moves = "
                        +current.level);

                System.out.println();

                printPath(current);

                return;

            }

            for(int i=0;i<4;i++){

                int newX=current.x+row[i];

                int newY=current.y+col[i];

                if(isSafe(newX,newY)){

                    Node child=createChild(
                            current,
                            newX,
                            newY
                    );

                    String key=
                            boardToString(child.board);

                    if(!visited.contains(key)){

                        visited.add(key);

                        pq.add(child);

                    }

                }

            }

        }

        System.out.println("Solution Not Found");

    }

    public static void main(String[] args){

        int[][] initial={

                {1,2,3,4},

                {5,6,7,8},

                {9,10,11,12},

                {13,0,14,15}

        };

        solve(initial,3,1);

    }

}
```

---

# Code Explanation

The algorithm performs the following steps.

```text
Create Root Node

↓

Compute Manhattan Distance

↓

Insert into Priority Queue

↓

Remove Lowest Cost State

↓

Goal Found?

↓

Yes → Print Solution

↓

No

↓

Generate Children

↓

Compute New Cost

↓

Insert Again

↓

Repeat
```

---

# Root Node Creation

```java
Node root =
        new Node(
                initial,
                blankX,
                blankY,
                blankX,
                blankY,
                0,
                null
        );
```

Initial values

```text
Level = 0

Parent = null

Cost = Manhattan Distance
```

---

# Priority Queue

```java
PriorityQueue<Node> pq =
        new PriorityQueue<>(
                Comparator.comparingInt(
                        node -> node.level + node.cost
                )
        );
```

Lowest

```text
f(n)
```

is always removed first.

---

# Search Loop

```java
while(!pq.isEmpty())
```

Continue until

- Goal found
- Queue becomes empty

---

# Goal Detection

```java
if(current.cost==0)
```

Since

```text
Manhattan Distance = 0
```

every tile is already in its goal position.

---

# Child Generation

```java
for(int i=0;i<4;i++)
```

Generate

```text
Up

Down

Left

Right
```

Each legal move creates one child.

---

# Duplicate State Removal

```java
if(!visited.contains(key))
```

Purpose

- Prevent cycles
- Avoid repeated exploration
- Improve efficiency

---

# Solution Path Reconstruction

Every node stores

```text
Parent Pointer
```

Example

```text
Goal

↓

Move 3

↓

Move 2

↓

Move 1

↓

Initial
```

The recursive function prints them in the reverse direction.

Final Output

```text
Initial

↓

Move 1

↓

Move 2

↓

Move 3

↓

Goal
```

---

# Sample Input

```text
1  2  3  4

5  6  7  8

9 10 11 12

13  0 14 15
```

Blank Position

```text
Row = 3

Column = 1
```

---

# Sample Output

```text
Goal State Reached

Minimum Moves = 2

1  2  3  4
5  6  7  8
9 10 11 12
13  0 14 15

1  2  3  4
5  6  7  8
9 10 11 12
13 14  0 15

1  2  3  4
5  6  7  8
9 10 11 12
13 14 15  0
```

---

# Dry Run

## Initial State

```text
g = 0

h = 2

f = 2
```

Queue

```text
Root
```

---

## Expand Root

Generate

```text
Left

Right

Up
```

Suppose

| Move | g | h | f |
|------|---|---|---|
| Left |1|4|5|
| Right|1|1|2|
| Up|1|5|6|

Queue

```text
Right

↓

Left

↓

Up
```

---

## Expand Right

Generate children.

One child becomes

```text
Goal State
```

Cost

```text
g = 2

h = 0

f = 2
```

Search stops.

---

# Complexity Analysis

## Time Complexity

Worst Case

```text
O(b^d)
```

where

- **b** = branching factor
- **d** = depth of optimal solution

The exponential complexity arises because multiple child states are generated at every level.

---

## Space Complexity

```text
O(b^d)
```

Reason

The priority queue and visited set may store a large number of generated states.

---

# Advantages of This Implementation

- Produces the **optimal solution** when using an admissible heuristic.
- Manhattan Distance is efficient and accurate.
- Avoids revisiting duplicate states.
- Uses a priority queue for best-first exploration.
- Prints the complete solution path.
- Modular and easy to extend.

---

# Notes for Exams

- **Branch:** Generate all valid child states.
- **Bound:** Compute `f(n) = g(n) + h(n)`.
- **Selection:** Expand the node with the minimum `f(n)`.
- **Termination:** Stop when `h(n) = 0` (goal reached).
- **Heuristic:** Manhattan Distance is preferred because it never overestimates the remaining cost.

---

# End of Part 3B

**Next (Part 4):**

- Advantages
- Disadvantages
- Limitations
- Edge Cases
- Correctness
- Comparison with BFS, DFS, A*, and IDA*
- Viva Questions
- University Exam Questions
- Quick Revision Cheat Sheet
- Summary

# 33. Advantages of Branch and Bound

Branch and Bound is one of the most effective algorithms for solving optimization and state-space search problems.

---

## 1. Produces Optimal Solution

The algorithm always expands the most promising node first.

When an **admissible heuristic** (such as Manhattan Distance) is used, the solution obtained is optimal.

```text
Minimum Moves

✓ Guaranteed
```

---

## 2. Avoids Brute Force Search

Brute Force

```text
Explore Every State

↓

Millions of Nodes

↓

Very Slow
```

Branch and Bound

```text
Explore Best States

↓

Ignore Bad States

↓

Fast Search
```

---

## 3. Reduces Search Space

Instead of exploring every branch,

the algorithm evaluates

```text
Cost

↓

Priority

↓

Expand
```

Poor branches are delayed or discarded.

---

## 4. Uses Heuristic Information

The heuristic estimates

```text
Remaining Distance
```

This makes the search much more intelligent.

Example

```text
State A

Estimated Cost = 18

------------------

State B

Estimated Cost = 6

------------------

Choose

↓

State B
```

---

## 5. Suitable for Large Problems

Useful for

- 15 Puzzle
- 8 Puzzle
- Assignment Problem
- Traveling Salesman
- Scheduling
- Robot Navigation

---

## 6. Flexible

Different heuristics can be used.

Examples

```text
Misplaced Tiles

Manhattan Distance

Linear Conflict

Pattern Database
```

Better heuristic

↓

Faster search

---

## 7. Systematic Search

The search never becomes random.

Each node follows

```text
Generate

↓

Evaluate

↓

Select Best

↓

Repeat
```

---

# 34. Disadvantages

Although powerful, Branch and Bound has several drawbacks.

---

## 1. High Memory Consumption

All generated nodes remain in the priority queue until explored.

```text
Root

↓

100 Nodes

↓

500 Nodes

↓

5000 Nodes

↓

Memory Increases
```

---

## 2. Worst Case is Exponential

Worst-case complexity

```text
O(b^d)
```

For deep puzzles,

execution time becomes very large.

---

## 3. Performance Depends on Heuristic

Poor heuristic

↓

Bad decisions

↓

More exploration

↓

Slow algorithm

---

## 4. Complex Implementation

Compared to DFS or BFS,

Branch and Bound requires

- Priority Queue
- Cost calculation
- Parent pointers
- Heuristic function

---

## 5. Not Suitable for Extremely Large Search Spaces

For very large puzzles,

memory may become the limiting factor.

---

# 35. Limitations

Some practical limitations include:

---

## Large Memory Requirement

Priority queue stores many states.

```text
Memory

↓

Can Overflow

↓

Out Of Memory
```

---

## Duplicate States

The same board configuration may appear through different paths.

Without a visited set,

the algorithm repeatedly explores identical states.

---

## Heuristic Quality

Good heuristic

```text
Fast Search
```

Poor heuristic

```text
Slow Search
```

---

## Difficult State Explosion

As depth increases,

number of states grows rapidly.

```text
Depth

1

↓

4

↓

16

↓

64

↓

256

↓

1024
```

---

# 36. Edge Cases

Every implementation should handle these situations.

---

## Case 1

Already Solved Puzzle

Input

```text
1 2 3 4

5 6 7 8

9 10 11 12

13 14 15 0
```

Output

```text
Minimum Moves = 0
```

---

## Case 2

Unsolvable Puzzle

Example

```text
1 2 3 4

5 6 7 8

9 10 11 12

13 15 14 0
```

The algorithm should first check solvability.

Otherwise,

it may search forever.

---

## Case 3

Blank in Corner

```text
0 1 2 3

4 5 6 7

8 9 10 11

12 13 14 15
```

Possible moves

```text
Down

Right
```

Only two legal moves.

---

## Case 4

Blank in Center

```text
1 2 3 4

5 0 6 7

8 9 10 11

12 13 14 15
```

Possible moves

```text
Up

Down

Left

Right
```

---

## Case 5

Deep Solution

Some puzzles require

```text
40+

50+

60+

moves
```

Memory usage increases significantly.

---

# 37. Correctness of the Algorithm

The algorithm is correct because:

1. Every legal move generates a valid child state.
2. Every generated state receives a valid cost estimate.
3. The priority queue always removes the minimum-cost node.
4. Goal detection occurs only when every tile reaches its target position.
5. Parent pointers reconstruct the shortest solution path.

Therefore,

the algorithm returns an **optimal solution** when the heuristic is admissible.

---

# 38. Comparison with Other Search Algorithms

| Feature | DFS | BFS | Branch & Bound | A* |
|----------|-----|-----|----------------|----|
| Uses Stack | ✓ | ✗ | ✗ | ✗ |
| Uses Queue | ✗ | ✓ | ✗ | ✗ |
| Uses Priority Queue | ✗ | ✗ | ✓ | ✓ |
| Uses Heuristic | ✗ | ✗ | ✓ | ✓ |
| Optimal Solution | ✗ | ✓ | ✓ | ✓ |
| Memory Usage | Low | High | High | High |
| Speed | Fast | Slow | Faster | Fastest (good heuristic) |

---

## Search Strategy Comparison

### DFS

```text
Root

↓

Deep

↓

Backtrack
```

---

### BFS

```text
Level 1

↓

Level 2

↓

Level 3
```

---

### Branch and Bound

```text
Lowest Cost

↓

Expand

↓

Repeat
```

---

### A*

```text
Lowest

g+h

↓

Expand
```

A* is often viewed as a specialized form of Branch and Bound with an admissible heuristic.

---

# 39. Frequently Asked Viva Questions

### Q1. What is Branch and Bound?

**Answer:**

A state-space search technique that finds the optimal solution using branching and cost-based pruning.

---

### Q2. Why is Manhattan Distance used?

**Answer:**

It estimates the minimum number of moves remaining and never overestimates the actual cost.

---

### Q3. What is Branch?

**Answer:**

Generating child states from the current state.

---

### Q4. What is Bound?

**Answer:**

An estimated cost used to decide which node should be explored next.

---

### Q5. Why is a Priority Queue used?

**Answer:**

To always expand the node with the minimum total cost.

---

### Q6. What is the cost function?

```text
f(n)=g(n)+h(n)
```

where

- `g(n)` = moves already made
- `h(n)` = Manhattan Distance

---

### Q7. What happens if the heuristic overestimates?

**Answer:**

The algorithm may lose its guarantee of finding the optimal solution.

---

### Q8. What is the worst-case complexity?

```text
Time : O(b^d)

Space : O(b^d)
```

---

# 40. University Exam Questions

## Short Answer Questions

1. Define Branch and Bound.
2. What is the 15-Puzzle problem?
3. What is Manhattan Distance?
4. What is a heuristic?
5. What is a live node?
6. What is a dead node?
7. Why is a priority queue used?
8. Define branching factor.
9. What is state-space search?
10. What is the bound function?

---

## Long Answer Questions

1. Explain Branch and Bound with the 15-Puzzle.
2. Explain the Manhattan Distance heuristic.
3. Write the Branch and Bound algorithm for the 15-Puzzle.
4. Write Java code to solve the 15-Puzzle using Branch and Bound.
5. Compare BFS, DFS, Branch and Bound, and A*.
6. Explain the time and space complexity.
7. Discuss the advantages and limitations of Branch and Bound.

---

# 41. Quick Revision Cheat Sheet

```text
Algorithm

↓

Branch and Bound

----------------------------

Data Structure

↓

Priority Queue

----------------------------

Evaluation Function

↓

f(n)=g(n)+h(n)

----------------------------

Heuristic

↓

Manhattan Distance

----------------------------

Branch

↓

Generate Children

----------------------------

Goal

↓

Cost = 0

----------------------------

Time Complexity

↓

O(b^d)

----------------------------

Space Complexity

↓

O(b^d)

----------------------------

Optimal

↓

Yes

----------------------------

State Representation

↓

4 × 4 Matrix

----------------------------

Blank Tile

↓

0
```

---

# 42. Summary

The **15-Puzzle** is a classic state-space search problem with a very large number of possible configurations. A brute-force search is computationally infeasible because the search space grows exponentially.

**Branch and Bound** addresses this by exploring the most promising states first using a **priority queue** and a **cost function**:

```text
f(n) = g(n) + h(n)
```

where:

- `g(n)` is the number of moves made so far.
- `h(n)` is the Manhattan Distance heuristic.

Key points:

- Each puzzle configuration is represented as a node.
- Child nodes are generated by moving the blank tile.
- The node with the smallest estimated total cost is expanded first.
- Parent pointers reconstruct the solution path.
- A visited set avoids revisiting duplicate states.
- Manhattan Distance provides an admissible heuristic, ensuring an optimal solution.

Although Branch and Bound requires significant memory and has exponential worst-case complexity, it is substantially more efficient than uninformed search methods for solving the 15-Puzzle.

---

# Final Revision Box

```text
✓ Branch = Generate Child States

✓ Bound = Estimated Cost

✓ Heuristic = Manhattan Distance

✓ Cost Function = g(n) + h(n)

✓ Data Structure = Priority Queue (Min Heap)

✓ State = 4 × 4 Matrix

✓ Blank Tile = 0

✓ Goal = Cost = 0

✓ Duplicate Detection = HashSet

✓ Time Complexity = O(b^d)

✓ Space Complexity = O(b^d)

✓ Guarantees Optimal Solution (with admissible heuristic)
```

---

**End of Document**

**Topic Completed:** **Branch and Bound – 15-Puzzle Problem**


