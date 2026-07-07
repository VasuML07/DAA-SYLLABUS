# Backtracking: N-Queens Problem (4-Queens & 8-Queens)

> **Course:** Design and Analysis of Algorithms (DAA)  
> **Topic:** Backtracking – N-Queens Problem  
> **Language:** Java  
> **Level:** Engineering Semester Exam Notes  
> **Document Type:** GitHub-Compatible Markdown

---

# Table of Contents

- [1. Introduction](#1-introduction)
- [2. What is Backtracking?](#2-what-is-backtracking)
- [3. Why Backtracking is Needed](#3-why-backtracking-is-needed)
- [4. When Should We Use Backtracking?](#4-when-should-we-use-backtracking)
- [5. Where is Backtracking Used?](#5-where-is-backtracking-used)
- [6. Advantages of Backtracking](#6-advantages-of-backtracking)
- [7. Disadvantages of Backtracking](#7-disadvantages-of-backtracking)
- [8. Basic Terminology](#8-basic-terminology)
- [9. Introduction to the N-Queens Problem](#9-introduction-to-the-n-queens-problem)
- [10. Problem Statement](#10-problem-statement)
- [11. Constraints of the N-Queens Problem](#11-constraints-of-the-n-queens-problem)
- [12. Understanding Queen Attacks](#12-understanding-queen-attacks)
- [13. Why Brute Force is Inefficient](#13-why-brute-force-is-inefficient)
- [14. Why Backtracking Works Better](#14-why-backtracking-works-better)
- [15. General Backtracking Algorithm](#15-general-backtracking-algorithm)
- [16. Flow of the N-Queens Algorithm](#16-flow-of-the-n-queens-algorithm)
- [17. High-Level Algorithm](#17-high-level-algorithm)
- [18. Time and Space Preview](#18-time-and-space-preview)
- [19. Exam Summary](#19-exam-summary)

---

# 1. Introduction

The **N-Queens Problem** is one of the most important applications of the **Backtracking Algorithm**.

The objective is simple:

> Place **N queens** on an **N × N chessboard** so that **no two queens attack each other**.

A queen in chess can attack horizontally, vertically, and diagonally.

Therefore, every queen placed on the board must satisfy three conditions:

- No two queens in the same row
- No two queens in the same column
- No two queens on the same diagonal

The problem is widely used for teaching:

- Recursion
- State-space search
- Constraint Satisfaction Problems (CSP)
- Artificial Intelligence
- Combinatorial Optimization

---

# 2. What is Backtracking?

Backtracking is a **recursive algorithmic technique** used to solve problems by building solutions **incrementally**.

Whenever the current partial solution becomes invalid, the algorithm **undoes (backtracks)** the last decision and tries another possibility.

Instead of exploring every possible arrangement completely, it abandons impossible paths early.

Think of it as:

```
Choose
↓

Check

↓

If valid
↓

Continue

↓

Else

↓

Undo

↓

Try another choice
```

This greatly reduces unnecessary computations.

---

## Formal Definition

Backtracking is a depth-first search technique where:

- A solution is constructed step by step.
- Every step is validated.
- Invalid states are discarded immediately.
- The algorithm returns to the previous decision whenever necessary.

---

# 3. Why Backtracking is Needed

Many computational problems involve exploring **all possible combinations**.

Examples:

- Sudoku
- Maze
- Crossword Puzzle
- Graph Coloring
- Hamiltonian Cycle
- Knight's Tour
- N-Queens

Trying every possibility using brute force becomes computationally expensive.

Backtracking improves efficiency by eliminating impossible solutions early.

Example:

Suppose the first queen attacks another queen.

Brute Force:

```
Continue exploring
all remaining positions.

Waste of computation.
```

Backtracking:

```
Immediately reject.

Return to previous position.

Try another column.
```

This process is called **pruning**.

---

# 4. When Should We Use Backtracking?

Backtracking is useful when:

- Multiple possible solutions exist.
- Decisions are made step by step.
- A wrong decision can be detected early.
- Constraints determine validity.
- We need one or all valid solutions.

Typical characteristics:

✔ Large search space

✔ Recursive nature

✔ Constraint checking

✔ Incremental solution building

---

## Do NOT Use Backtracking When

Avoid backtracking if:

- Dynamic Programming provides overlapping subproblems.
- Greedy gives an optimal solution.
- Simple graph traversal solves the problem.
- Mathematical formula exists.

Example:

Finding shortest path:

Use

- BFS
- Dijkstra

instead of backtracking.

---

# 5. Where is Backtracking Used?

Backtracking appears in many real-world applications.

| Problem | Usage |
|----------|------|
| Sudoku Solver | Puzzle solving |
| Crossword Generator | Word placement |
| Password Generation | Combination search |
| Robot Navigation | Path finding |
| Graph Coloring | Compiler optimization |
| Circuit Design | Constraint solving |
| AI Planning | State-space search |
| Timetable Scheduling | Resource allocation |
| Chess Engines | Move generation |
| N-Queens | Constraint satisfaction |

---

# 6. Advantages of Backtracking

Advantages include:

- Easy recursive implementation
- Finds all possible solutions
- Eliminates invalid branches early
- Saves computation compared to brute force
- Flexible for many combinatorial problems

---

## Advantages Illustration

```
Brute Force

1000 possibilities

↓

Checks all

↓

Slow



Backtracking

1000 possibilities

↓

Reject invalid after 10

↓

Only 120 explored

↓

Much faster
```

---

# 7. Disadvantages of Backtracking

Although better than brute force, backtracking is still expensive.

Disadvantages:

- Worst-case exponential complexity
- Large recursion depth
- Slow for very large N
- May explore many branches before success

---

# 8. Basic Terminology

## State

Current configuration.

Example:

```
Q . . .

. . . .

. . . .

. . . .
```

---

## Decision

Choosing where to place the next queen.

---

## Constraint

A queen cannot attack another queen.

---

## Pruning

Discarding an invalid branch immediately.

---

## Backtracking

Undoing the last decision.

---

## Solution

A complete board satisfying all constraints.

---

# 9. Introduction to the N-Queens Problem

Suppose:

```
N = 4
```

Board:

```
□ □ □ □

□ □ □ □

□ □ □ □

□ □ □ □
```

Goal:

Place four queens.

```
Q

Q

Q

Q
```

without attacking one another.

---

## Important Rule

Only one queen is placed in each row.

Therefore,

Instead of searching the whole board,

we only decide:

> Which column should the queen occupy in the current row?

This reduces the search space significantly.

---

# 10. Problem Statement

Given an integer **N**, place **N queens** on an **N × N** chessboard such that:

- Every row has exactly one queen.
- Every column has exactly one queen.
- No two queens share the same diagonal.

Return:

- One solution
- Multiple solutions
- Or simply determine whether a solution exists.

---

## Mathematical Representation

For queens located at:

```
(r1,c1)

(r2,c2)
```

Conflict occurs if:

### Same Row

```
r1 == r2
```

### Same Column

```
c1 == c2
```

### Same Diagonal

```
|r1-r2| = |c1-c2|
```

---

# 11. Constraints of the N-Queens Problem

For every queen:

```
Exactly one queen per row
```

```
Exactly one queen per column
```

```
No diagonal attack
```

These constraints must remain true after every placement.

If any constraint fails,

the algorithm immediately backtracks.

---

# 12. Understanding Queen Attacks

Consider one queen.

```
. . . .

. Q . .

. . . .

. . . .
```

The queen attacks:

Horizontal

```
× × × ×
```

Vertical

```
×

×

Q

×

×
```

Diagonals

```
\

  \

    Q

  /

/
```

Complete visualization:

```
× . × .

. × . ×

× Q × ×

. × . ×
```

Any new queen placed on a marked square is invalid.

---

# 13. Why Brute Force is Inefficient

Without backtracking:

```
Try every arrangement.

Check validity later.
```

Number of arrangements:

```
N^N
```

or even

```
N!
```

depending on implementation.

Most arrangements are invalid.

Example:

```
Q . . .

Q . . .

. . . .

. . . .
```

Already invalid,

yet brute force may continue exploring.

This wastes enormous computation.

---

# 14. Why Backtracking Works Better

Backtracking checks validity immediately.

Example:

```
Row 1

Q . . .

↓

Row 2

Q . . .

↓

Conflict

↓

Undo

↓

Try another column
```

Instead of exploring thousands of impossible boards,

the algorithm rejects them instantly.

---

## Search Tree Concept

```
Start

│

├── Column 1

│      │

│      ├── Valid

│      │

│      └── Invalid

│

├── Column 2

│

├── Column 3

│

└── Column 4
```

Invalid branches stop growing.

Only promising branches continue.

---

# 15. General Backtracking Algorithm

Generic structure:

```
Solve(State)

|

|-- Is solution complete?

|        |

|        Yes

|        |

|      Print solution

|

|-- Otherwise

        |

        Try every possible choice

        |

        If choice is safe

              |

              Make choice

              |

              Solve(next state)

              |

              Undo choice
```

---

# 16. Flow of the N-Queens Algorithm

```
Start

↓

Row = 0

↓

Try Column 0

↓

Safe?

↓

Yes

↓

Place Queen

↓

Next Row

↓

Safe?

↓

No

↓

Backtrack

↓

Remove Queen

↓

Try Next Column

↓

Repeat

↓

Finished
```

---

# 17. High-Level Algorithm

```
Place queen in first row.

↓

Check safety.

↓

If safe,

go to next row.

↓

Repeat.

↓

If no safe position exists,

remove previous queen.

↓

Try another column.

↓

Continue until all rows are filled.

↓

Print solution.
```

---

# 18. Time and Space Preview

| Complexity | Value |
|------------|-------|
| Worst Case Time | O(N!) |
| Best Case | Better due to pruning |
| Auxiliary Space | O(N) recursion |
| Board Storage | O(N²) |

Although the worst case remains exponential, pruning dramatically reduces the practical search space.

---

# 19. Exam Summary

### Definition

Backtracking is a recursive problem-solving technique that incrementally builds a solution and abandons any partial solution as soon as it violates the problem constraints.

### Key Idea

- Choose
- Check
- Recurse
- Backtrack
- Try another choice

### N-Queens Goal

Place **N queens** on an **N × N** chessboard such that:

- No two queens share the same row.
- No two queens share the same column.
- No two queens share the same diagonal.

### Why Use Backtracking?

- Eliminates invalid solutions early.
- Reduces unnecessary computation.
- Efficient for constraint satisfaction problems.

### Common Exam Questions

1. Define Backtracking.
2. Explain the N-Queens problem.
3. Why is brute force inefficient?
4. Explain pruning.
5. Write the general backtracking algorithm.
6. State the time complexity of the N-Queens problem.
7. List real-world applications of backtracking.

---
# 20. Complete 4-Queens Walkthrough (Step-by-Step)

Now let's solve the **4-Queens Problem** using the Backtracking algorithm.

We place **one queen per row**.

For every row:

1. Try each column.
2. Check whether the position is safe.
3. If safe, place the queen.
4. Move to the next row.
5. If no column is safe, remove the previous queen (Backtrack).

---

# Initial Empty Board

```
Rows↓

      C0 C1 C2 C3

R0    .  .  .  .

R1    .  .  .  .

R2    .  .  .  .

R3    .  .  .  .
```

Legend

```
Q = Queen

. = Empty Cell

X = Unsafe Position
```

---

# Step 1 — Place Queen in Row 0

Try Column 0.

```
      C0 C1 C2 C3

R0    Q  .  .  .

R1    .  .  .  .

R2    .  .  .  .

R3    .  .  .  .
```

Safe?

```
YES
```

Proceed to Row 1.

---

# Step 2 — Try Row 1

Try Column 0.

```
Conflict

Same Column
```

Rejected.

```
Q . . .

Q . . .
```

---

Try Column 1.

```
Diagonal Conflict
```

```
Q . . .

. Q . .
```

Rejected.

---

Try Column 2.

```
Safe
```

Board becomes

```
      C0 C1 C2 C3

R0    Q  .  .  .

R1    .  .  Q  .

R2    .  .  .  .

R3    .  .  .  .
```

Move to Row 2.

---

# Step 3 — Row 2

Try every column.

Column 0

```
Conflict

Same Column
```

Rejected.

---

Column 1

```
Diagonal Conflict
```

Rejected.

---

Column 2

```
Same Column
```

Rejected.

---

Column 3

```
Diagonal Conflict
```

Rejected.

---

Result

```
No valid position exists.
```

So we must **Backtrack**.

---

# Backtracking

Remove Queen from Row 1.

```
      C0 C1 C2 C3

R0    Q  .  .  .

R1    .  .  .  .

R2    .  .  .  .

R3    .  .  .  .
```

Now continue trying remaining columns in Row 1.

---

# Step 4 — Try Row 1 Column 3

```
      C0 C1 C2 C3

R0    Q  .  .  .

R1    .  .  .  Q

R2    .  .  .  .

R3    .  .  .  .
```

Safe?

```
YES
```

Proceed.

---

# Step 5 — Place Queen in Row 2

Try Column 0

Conflict

```
Same Column
```

Rejected.

---

Column 1

Safe.

```
      C0 C1 C2 C3

R0    Q  .  .  .

R1    .  .  .  Q

R2    .  Q  .  .

R3    .  .  .  .
```

Proceed.

---

# Step 6 — Row 3

Try Column 0

```
Same Column
```

Reject.

---

Column 1

```
Same Column
```

Reject.

---

Column 2

```
Diagonal Conflict
```

Reject.

---

Column 3

```
Same Column
```

Reject.

Again,

```
No solution.
```

Backtrack.

---

Remove Queen from Row 2.

```
      C0 C1 C2 C3

R0    Q  .  .  .

R1    .  .  .  Q

R2    .  .  .  .

R3    .  .  .  .
```

Continue trying remaining columns.

Column 2

Diagonal Conflict.

Column 3

Same Column.

No options.

Backtrack again.

---

Remove Queen from Row 1.

```
      C0 C1 C2 C3

R0    Q  .  .  .

R1    .  .  .  .

R2    .  .  .  .

R3    .  .  .  .
```

No more columns remain.

Therefore,

Backtrack to Row 0.

---

# Step 7 — Move First Queen

Instead of Column 0,

place first queen in Column 1.

```
      C0 C1 C2 C3

R0    .  Q  .  .

R1    .  .  .  .

R2    .  .  .  .

R3    .  .  .  .
```

Proceed.

---

# Step 8 — Row 1

Column 0

Diagonal Conflict

Reject.

Column 1

Same Column

Reject.

Column 2

Diagonal Conflict

Reject.

Column 3

Safe.

```
      C0 C1 C2 C3

R0    .  Q  .  .

R1    .  .  .  Q

R2    .  .  .  .

R3    .  .  .  .
```

---

# Step 9 — Row 2

Column 0

Safe.

```
      C0 C1 C2 C3

R0    .  Q  .  .

R1    .  .  .  Q

R2    Q  .  .  .

R3    .  .  .  .
```

Proceed.

---

# Step 10 — Row 3

Column 0

Same Column

Reject.

Column 1

Same Column

Reject.

Column 2

Safe.

```
      C0 C1 C2 C3

R0    .  Q  .  .

R1    .  .  .  Q

R2    Q  .  .  .

R3    .  .  Q  .
```

All rows filled.

Solution Found.

---

# First Valid Solution

```
      C0 C1 C2 C3

R0    .  Q  .  .

R1    .  .  .  Q

R2    Q  .  .  .

R3    .  .  Q  .
```

Column positions

```
[1,3,0,2]
```

---

# Verification

Rows

```
✓ One queen each
```

Columns

```
✓ One queen each
```

Diagonals

```
✓ No attacks
```

Hence,

```
Valid Solution
```

---

# Second Valid Solution

If the search continues,

another solution is discovered.

```
      C0 C1 C2 C3

R0    .  .  Q  .

R1    Q  .  .  .

R2    .  .  .  Q

R3    .  Q  .  .
```

Column representation

```
[2,0,3,1]
```

---

# Complete Search Tree

```
Start

│

├── C0

│   │

│   ├── Dead End

│   │

│   └── Backtrack

│

├── C1

│   │

│   ├── Solution 1

│   │

│   └── Continue

│

├── C2

│   │

│   ├── Solution 2

│   │

│   └── Continue

│

└── C3

    │

    └── Dead End
```

---

# Backtracking Tree Visualization

```
Row0

│

├── Col0

│     │

│     ├── Col2

│     │      │

│     │      X

│     │

│     └── Col3

│            │

│            X

│

├── Col1

│      │

│      ├── Col3

│      │

│      ├── Col0

│      │

│      └── Col2

│

│      SUCCESS

│

├── Col2

│

│      SUCCESS

│

└── Col3

       X
```

---

# Understanding Pruning

Suppose we reach this board.

```
Q . . .

. . Q .

. . . .

. . . .
```

Trying Row 2.

```
Q . . .

. . Q .

Q . . .
```

Immediately rejected.

Reason

```
Same Column
```

No need to search further.

Another example

```
Q . . .

. . Q .

. Q . .
```

Rejected immediately.

Reason

```
Diagonal attack
```

This early rejection is called **Pruning**.

---

# Why Backtracking Saves Time

Without pruning

```
Explore

↓

Check

↓

Reject
```

With pruning

```
Conflict

↓

Reject immediately

↓

Search another branch
```

Large portions of the search tree are never explored.

---

# Recursion Stack During Search

```
solve(0)

↓

solve(1)

↓

solve(2)

↓

Dead End

↓

Return

↓

solve(1)

↓

New Choice

↓

solve(2)

↓

solve(3)

↓

Solution
```

The recursion naturally handles moving forward and backward.

---

# Key Observations for 4-Queens

- There are **2 distinct solutions**.
- Many board configurations fail early.
- Backtracking avoids exploring impossible configurations completely.
- The algorithm systematically explores every valid possibility.

---

# Exam Points to Remember

- Place **one queen per row**.
- Check **column**, **left diagonal**, and **right diagonal** before placing a queen.
- If no safe position exists, **remove the previous queen**.
- Continue until all rows are successfully filled.
- The 4-Queens problem has exactly **2 valid solutions**.

---

**Next Part:** Part 3 covers the **8-Queens problem**, differences from 4-Queens, edge cases (N=1,2,3), complexity analysis, scalability, and performance limitations.

# 21. The 8-Queens Problem

The **8-Queens Problem** is the generalized and most well-known version of the N-Queens problem.

Instead of placing **4 queens**, we place **8 queens** on an **8 × 8 chessboard** such that:

- No two queens are in the same row.
- No two queens are in the same column.
- No two queens attack each other diagonally.

The same **backtracking algorithm** used for 4-Queens works for 8-Queens. The only change is the board size.

---

# 22. Initial 8 × 8 Board

```
      C0 C1 C2 C3 C4 C5 C6 C7

R0    .  .  .  .  .  .  .  .

R1    .  .  .  .  .  .  .  .

R2    .  .  .  .  .  .  .  .

R3    .  .  .  .  .  .  .  .

R4    .  .  .  .  .  .  .  .

R5    .  .  .  .  .  .  .  .

R6    .  .  .  .  .  .  .  .

R7    .  .  .  .  .  .  .  .
```

---

# 23. Example of One Valid 8-Queens Solution

One valid arrangement is:

```
      C0 C1 C2 C3 C4 C5 C6 C7

R0    Q  .  .  .  .  .  .  .

R1    .  .  .  .  Q  .  .  .

R2    .  .  .  .  .  .  .  Q

R3    .  .  .  .  .  Q  .  .

R4    .  .  Q  .  .  .  .  .

R5    .  .  .  .  .  .  Q  .

R6    .  Q  .  .  .  .  .  .

R7    .  .  .  Q  .  .  .  .
```

Column representation:

```
[0,4,7,5,2,6,1,3]
```

Each row contains exactly one queen, and no two queens attack each other.

---

# 24. How the Algorithm Solves 8-Queens

The algorithm follows the same recursive process:

```
Row 0
↓

Try every column
↓

Place queen if safe
↓

Move to Row 1
↓

Repeat
↓

Dead end?

↓

Backtrack
↓

Remove previous queen
↓

Try next column
```

This continues until:

- All 8 rows are filled (solution found), or
- Every possibility has been explored.

---

# 25. Why Backtracking is Essential for 8-Queens

Consider this partial board:

```
R0    Q . . . . . . .

R1    . . . Q . . . .

R2    . . . . . Q . .

R3    . Q . . . . . .

R4    . . . . . . . .

R5    . . . . . . . .

R6    . . . . . . . .

R7    . . . . . . . .
```

Suppose no valid position exists in **Row 4**.

A brute-force algorithm would continue checking invalid combinations.

Backtracking does this instead:

```
No safe position in Row 4

↓

Remove queen from Row 3

↓

Try another column

↓

Continue searching
```

Only the affected branch is reconsidered.

---

# 26. Search Tree for 8-Queens

```
                     Start
                       |
        --------------------------------
       /      /      /      /          \
     C0     C1     C2     C3        ... C7
      |
   Row 1
      |
  -----------------
  |   |   |   |
 C0  C1  C2  C3 ...
      |
   Safe?
   /   \
 Yes    No
 |        |
Next    Prune
Row
```

Every level represents a row of the chessboard.

---

# 27. Pruning in 8-Queens

Suppose the current board is:

```
R0    Q . . . . . . .

R1    . . Q . . . . .

R2    . . . . Q . . .

R3    . . . . . . . .

R4    . . . . . . . .

R5    . . . . . . . .

R6    . . . . . . . .

R7    . . . . . . . .
```

Trying to place the next queen:

```
Candidate Position

↓

Attacked?

↓

YES

↓

Reject Immediately

↓

Try Next Column
```

The algorithm never explores children of an invalid state.

---

# 28. State-Space Tree

```
                    Root

                      |

               Row 0 Choices

          /      |      |      \

        C0      C1     C2      C3

         |

     Row 1 Choices

     /   |   |   \

   Safe Safe X Safe

         |

     Row 2 Choices

         |

      Continue

         |

     Solution
```

Nodes marked **X** are pruned.

---

# 29. Edge Cases

## Case 1 — N = 1

Board:

```
Q
```

Only one queen.

No conflicts.

**Number of Solutions:** 1

---

## Case 2 — N = 2

```
. .

. .
```

Try every arrangement:

```
Q .

. Q
```

Diagonal attack.

```
. Q

Q .
```

Diagonal attack.

No arrangement satisfies the constraints.

**Number of Solutions:** 0

---

## Case 3 — N = 3

Example:

```
Q . .

. . Q

. Q .
```

Queens attack diagonally.

Trying every possibility still results in conflicts.

**Number of Solutions:** 0

---

## Case 4 — N = 4

First board size with a non-trivial solution.

**Number of Solutions:** 2

---

## Case 5 — N = 8

Most famous version.

**Number of Distinct Solutions:** 92

(There are 12 unique solutions if rotations and reflections are considered equivalent.)

---

# 30. Why N = 2 and N = 3 Have No Solution

### N = 2

```
Q .

. Q
```

Diagonal attack.

```
. Q

Q .
```

Diagonal attack.

Every possible arrangement fails.

---

### N = 3

```
Q . .

. Q .

. . Q
```

Main diagonal conflict.

Another arrangement:

```
. Q .

Q . .

. . Q
```

Still results in diagonal conflicts.

No valid placement exists.

---

# 31. Time Complexity

The algorithm places one queen in each row.

For every row, multiple columns may be explored.

Worst-case complexity:

```
O(N!)
```

Reason:

- First row → N choices
- Second row → at most N − 1 choices
- Third row → at most N − 2 choices
- ...

Total possibilities:

```
N × (N−1) × (N−2) × ...

= N!
```

Pruning significantly reduces the number of states explored in practice, but the worst-case complexity remains exponential.

---

# 32. Space Complexity

## Recursion Stack

Maximum recursive depth:

```
N
```

Therefore:

```
O(N)
```

## Chessboard Storage

If a full board is stored:

```
N × N
```

Space:

```
O(N²)
```

Many optimized implementations store only the column position of each queen:

```
int[] queens = new int[N];
```

This reduces board storage to:

```
O(N)
```

---

# 33. Growth of the Search Space

| N | Solutions | Search Difficulty |
|---|----------:|-------------------|
| 1 | 1 | Very Easy |
| 2 | 0 | Impossible |
| 3 | 0 | Impossible |
| 4 | 2 | Easy |
| 5 | 10 | Moderate |
| 6 | 4 | Moderate |
| 7 | 40 | Hard |
| 8 | 92 | Hard |
| 10 | 724 | Very Hard |
| 12 | 14,200 | Very Large Search Space |

As **N** increases, the search space grows rapidly, making efficient pruning essential.

---

# 34. Limitations of Backtracking

Although backtracking is much better than brute force, it has limitations:

- Exponential worst-case running time.
- Large search tree for high values of **N**.
- Recursive overhead.
- Performance decreases rapidly as **N** grows.
- May revisit many partial configurations before finding a solution.

---

# 35. Advantages of Backtracking for N-Queens

- Simple recursive implementation.
- Guarantees finding a solution if one exists.
- Can enumerate all valid solutions.
- Eliminates invalid branches early through pruning.
- Applicable to many other constraint satisfaction problems.

---

# 36. Comparison: Brute Force vs Backtracking

| Feature | Brute Force | Backtracking |
|---------|-------------|--------------|
| Explores every possibility | Yes | No |
| Rejects invalid states early | No | Yes |
| Uses recursion | Optional | Yes |
| Employs pruning | No | Yes |
| Practical for larger N | No | More practical |
| Finds all solutions | Yes | Yes |

---

# 37. Exam Tips

Remember these key points:

- Place **one queen per row**.
- Check **column**, **left diagonal**, and **right diagonal** before placement.
- If no valid position exists, **backtrack** by removing the last queen.
- Continue until all rows are processed.
- Worst-case time complexity is **O(N!)**.
- Auxiliary recursion space is **O(N)**.
- **N = 2** and **N = 3** have no solutions.
- **N = 4** has **2** solutions.
- **N = 8** has **92** solutions.

---

# 38. Quick Revision

```
Choose Position
        ↓
Check Safety
        ↓
Safe?
   /        \
 Yes         No
 |           |
Place      Try Next Column
 |
Next Row
 |
Dead End?
 |
Yes
 |
Backtrack
 |
Remove Queen
 |
Continue Search
```

This flow summarizes the complete backtracking strategy for solving the N-Queens problem.

---

**Next Part:** Part 4 includes the complete **Java implementation**, detailed code explanation, sample outputs for 4-Queens and 8-Queens, dry run, applications, viva questions, interview questions, and exam revision notes.

# 39. Java Implementation of N-Queens (Backtracking)

The following Java program solves the **N-Queens Problem** using the **Backtracking Algorithm**.

The program is **parameterized**, meaning it works for:

- 4-Queens
- 8-Queens
- Any value of **N ≥ 1**

---

## Complete Java Program

```java
public class NQueens {

    // Size of chessboard
    private int N;

    // Stores the column position of each queen
    private int[] board;

    public NQueens(int n) {
        N = n;
        board = new int[N];

        // Initialize all positions to -1
        for (int i = 0; i < N; i++) {
            board[i] = -1;
        }
    }

    // Check whether a queen can be placed
    private boolean isSafe(int row, int col) {

        for (int i = 0; i < row; i++) {

            // Same column
            if (board[i] == col)
                return false;

            // Same diagonal
            if (Math.abs(board[i] - col) == Math.abs(i - row))
                return false;
        }

        return true;
    }

    // Recursive backtracking function
    private boolean solve(int row) {

        // Base Case
        if (row == N)
            return true;

        // Try every column
        for (int col = 0; col < N; col++) {

            if (isSafe(row, col)) {

                // Place queen
                board[row] = col;

                // Solve next row
                if (solve(row + 1))
                    return true;

                // Backtrack
                board[row] = -1;
            }
        }

        return false;
    }

    // Print solution
    private void printBoard() {

        System.out.println("\nSolution for " + N + "-Queens\n");

        for (int i = 0; i < N; i++) {

            for (int j = 0; j < N; j++) {

                if (board[i] == j)
                    System.out.print(" Q ");
                else
                    System.out.print(" . ");
            }

            System.out.println();
        }
    }

    public void start() {

        if (solve(0))
            printBoard();
        else
            System.out.println("No Solution Exists");
    }

    public static void main(String[] args) {

        System.out.println("4 Queens");
        NQueens q4 = new NQueens(4);
        q4.start();

        System.out.println("\n--------------------------");

        System.out.println("8 Queens");
        NQueens q8 = new NQueens(8);
        q8.start();
    }
}
```

---

# 40. Code Explanation

## Step 1 – Create the Board

```java
private int[] board;
```

Instead of storing the complete chessboard,

we store only the column index of every queen.

Example

```
board = [1,3,0,2]
```

means

```
Row 0 → Column 1

Row 1 → Column 3

Row 2 → Column 0

Row 3 → Column 2
```

This reduces memory usage.

---

## Step 2 – Safety Checking

```java
isSafe(row,col)
```

Checks

- Same column
- Left diagonal
- Right diagonal

If all checks pass

```
Return true
```

otherwise

```
Return false
```

---

## Step 3 – Recursive Function

```java
solve(row)
```

tries

```
Column 0

↓

Column 1

↓

Column 2

↓

Column 3

...
```

Whenever a safe column is found,

the queen is placed.

---

## Step 4 – Base Case

```java
if(row==N)
```

means

All rows have queens.

Therefore,

```
Solution Found
```

---

## Step 5 – Backtracking

If future rows cannot place queens

```
Remove current queen

↓

Try next column
```

This is the essence of Backtracking.

---

# 41. Dry Run (4-Queens)

Initial board

```
. . . .

. . . .

. . . .

. . . .
```

---

Place first queen

```
Q . . .

. . . .

. . . .

. . . .
```

---

Place second queen

```
Q . . .

. . Q .

. . . .

. . . .
```

---

No valid position exists in Row 2

```
Backtrack

↓

Remove Queen
```

```
Q . . .

. . . .

. . . .

. . . .
```

Try another column

Continue recursively

Eventually

```
. Q . .

. . . Q

Q . . .

. . Q .
```

Solution Found.

---

# 42. Sample Output (4-Queens)

```
Solution for 4-Queens

. Q . .

. . . Q

Q . . .

. . Q .
```

---

# 43. Sample Output (8-Queens)

One possible output

```
Q . . . . . . .

. . . . Q . . .

. . . . . . . Q

. . . . . Q . .

. . Q . . . . .

. . . . . . Q .

. Q . . . . . .

. . . Q . . . .
```

Different valid outputs are also correct because the algorithm may explore columns in a different order.

---

# 44. Complexity Analysis

| Parameter | Complexity |
|-----------|------------|
| Worst-case Time | **O(N!)** |
| Best Case | Depends on pruning |
| Auxiliary Space | **O(N)** |
| Board Storage | **O(N)** (using column array) |
| Full Board Representation | **O(N²)** |

---

# 45. Why This Algorithm Works

The algorithm maintains the following invariant:

- Each recursive call corresponds to placing one queen in a new row.
- A queen is placed only if it does not conflict with previously placed queens.
- If a row has no valid position, the algorithm backtracks to revise earlier choices.

This guarantees that every reported configuration is a valid solution.

---

# 46. Applications of Backtracking

Backtracking is widely used in constraint satisfaction and combinatorial search problems.

| Problem | Description |
|---------|-------------|
| N-Queens | Chessboard constraint satisfaction |
| Sudoku Solver | Fill numbers under constraints |
| Maze Solving | Find a valid path |
| Rat in a Maze | Grid traversal |
| Graph Coloring | Assign colors without conflicts |
| Hamiltonian Cycle | Visit every vertex once |
| Knight's Tour | Chess traversal |
| Crossword Puzzle | Word placement |
| Cryptarithmetic | Letter-digit assignments |
| Timetable Scheduling | Resource allocation |

---

# 47. Advantages

- Easy to implement using recursion.
- Finds one or all valid solutions.
- Prunes invalid search branches.
- Reduces unnecessary computation compared to brute force.
- Suitable for many combinatorial problems.

---

# 48. Disadvantages

- Exponential worst-case complexity.
- Performance decreases rapidly as **N** increases.
- Recursive calls consume stack space.
- Not suitable for very large search spaces without optimization.

---

# 49. Frequently Asked Viva Questions

### Q1. What is Backtracking?

A recursive algorithmic technique that builds a solution incrementally and abandons a partial solution as soon as it becomes invalid.

---

### Q2. Why is Backtracking preferred over Brute Force?

Because it prunes invalid branches early, reducing unnecessary computations.

---

### Q3. Why do we place one queen per row?

It guarantees that no two queens share the same row and simplifies the search.

---

### Q4. What conditions must be checked before placing a queen?

- Same column
- Left diagonal
- Right diagonal

---

### Q5. What is pruning?

The process of stopping the exploration of an invalid partial solution.

---

### Q6. What is the worst-case time complexity?

```
O(N!)
```

---

### Q7. What is the auxiliary space complexity?

```
O(N)
```

due to the recursion stack.

---

### Q8. How many solutions exist for 4-Queens?

```
2
```

---

### Q9. How many solutions exist for 8-Queens?

```
92
```

---

### Q10. Which values of N have no solution?

```
N = 2

N = 3
```

---

# 50. Interview Questions

1. Explain the Backtracking algorithm.
2. How does pruning improve efficiency?
3. Write the recursive algorithm for N-Queens.
4. Explain the `isSafe()` function.
5. Compare Brute Force and Backtracking.
6. What is the recursion tree for N-Queens?
7. Can the algorithm print all solutions instead of one?
8. How can space usage be optimized?
9. Why is the time complexity factorial?
10. Mention real-world applications of Backtracking.

---

# 51. Common Exam Mistakes

- Forgetting to check diagonal conflicts.
- Checking only rows and columns.
- Not removing the queen during backtracking.
- Missing the recursive base case.
- Incorrect diagonal condition:
  ```java
  Math.abs(board[i] - col) == Math.abs(i - row)
  ```
- Confusing **O(N²)** with **O(N!)** time complexity.

---

# 52. Quick Revision Sheet

## Definition

Backtracking is a recursive technique that constructs solutions step by step and abandons invalid partial solutions immediately.

---

## Algorithm Steps

```
Choose

↓

Check

↓

Place Queen

↓

Recursive Call

↓

Failure?

↓

Backtrack

↓

Try Next Choice
```

---

## Conditions for Safe Placement

- No same row (handled by placing one queen per row).
- No same column.
- No same left diagonal.
- No same right diagonal.

---

## Complexity

| Metric | Value |
|--------|-------|
| Time | **O(N!)** |
| Auxiliary Space | **O(N)** |
| Board Storage | **O(N)** using a column array |

---

## Key Facts

- N = 1 → 1 solution
- N = 2 → No solution
- N = 3 → No solution
- N = 4 → 2 solutions
- N = 8 → 92 solutions

---

# 53. Conclusion

The **N-Queens Problem** is a classic example of the **Backtracking Algorithm** and demonstrates how recursive search combined with pruning efficiently solves constraint satisfaction problems.

The key idea is to place queens one row at a time, verify safety before each placement, and backtrack whenever a conflict is encountered. Although the worst-case time complexity is **O(N!)**, pruning significantly reduces the practical search space, making backtracking far more efficient than naive brute-force exploration.

Understanding the N-Queens problem provides a strong foundation for solving many other recursive and backtracking problems such as Sudoku, Graph Coloring, Hamiltonian Cycle, Rat in a Maze, and Knight's Tour.

---

# One-Page Exam Revision

```
BACKTRACKING
    │
    ▼
Choose a Position
    │
    ▼
Is it Safe?
 ┌───────┴────────┐
 │                │
Yes              No
 │                │
 ▼                ▼
Place Queen   Try Next Column
 │
 ▼
Next Row
 │
 ▼
All Rows Filled?
 ┌───────┴────────┐
 │                │
Yes              No
 │                │
 ▼                ▼
Print Solution  Backtrack
                 │
                 ▼
          Remove Queen
                 │
                 ▼
          Try Another Choice
```

> **Exam Tip:** Remember the three safety checks (column, left diagonal, right diagonal), the recursive base case (`row == N`), and the worst-case time complexity **O(N!)**. These are the most frequently tested concepts.
**Next Part:** Part 2 covers the complete **4-Queens problem walkthrough**, including detailed ASCII board diagrams, step-by-step queen placements, backtracking decisions, pruning, recursion tree, and construction of all valid 4-Queens solutions.

