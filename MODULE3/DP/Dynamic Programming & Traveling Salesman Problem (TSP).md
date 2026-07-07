# Dynamic Programming & Traveling Salesman Problem (TSP) — Complete Study Guide (Design and Analysis of Algorithms)

> Engineering Semester Exam Notes (GitHub Compatible)

---

# Table of Contents

- [1. Why Study Dynamic Programming and TSP?](#1-why-study-dynamic-programming-and-tsp)
- [2. When to Use Dynamic Programming and TSP](#2-when-to-use-dynamic-programming-and-tsp)
- [3. Where is DP-TSP Used?](#3-where-is-dp-tsp-used)
- [4. How to Solve TSP Using Dynamic Programming](#4-how-to-solve-tsp-using-dynamic-programming)
- [5. DP-TSP Algorithm Explanation](#5-dp-tsp-algorithm-explanation)
- [6. Visual Demonstrations](#6-visual-demonstrations)
- [7. Limitations and Edge Cases](#7-limitations-and-edge-cases)
- [8. Java Implementation](#8-java-implementation)
- [9. Complexity Analysis](#9-complexity-analysis)
- [10. Advantages and Disadvantages](#10-advantages-and-disadvantages)
- [11. Frequently Asked Exam Questions](#11-frequently-asked-exam-questions)
- [12. Key Points to Remember](#12-key-points-to-remember)

---

# 1. Why Study Dynamic Programming and TSP?

## Why Dynamic Programming?

Dynamic Programming (DP) is a powerful optimization technique used when:

- Problems contain **overlapping subproblems**
- Problems have **optimal substructure**
- Recursive solutions perform repeated calculations

Instead of solving the same subproblem repeatedly, DP stores previously computed results (memoization or tabulation).

### Motivation

Without DP:

```
Repeated Computation

Solve(A)
 ├── Solve(B)
 │     ├── Solve(D)
 │     └── Solve(E)
 └── Solve(C)
       ├── Solve(D)   ← repeated
       └── Solve(E)   ← repeated
```

With DP:

```
Solve(D) Once
↓

Store Result

↓

Reuse Whenever Needed
```

---

## Why Traveling Salesman Problem?

The Traveling Salesman Problem asks:

> Find the minimum-cost route that visits every city exactly once and returns to the starting city.

Real-world motivation:

- Fuel savings
- Reduced delivery time
- Efficient scheduling
- Network optimization

---

## Problem Statement

Given

- N cities
- Cost between every pair

Find

- Minimum Hamiltonian Cycle.

---

Example

```
      A
    / | \
 10/  |15\
  /   |   \
 B----C----D
 35 30 20
```

Goal

```
A → B → C → D → A

Minimum Cost
```

---

# 2. When to Use Dynamic Programming and TSP

Use DP when:

✅ Optimal substructure exists

```
Optimal Solution

↓

Optimal Smaller Solutions
```

---

✅ Overlapping subproblems

Example:

```
Solve(ABC)

Needs

Solve(AB)
Solve(BC)
Solve(AB) again

↓

Store once
Reuse
```

---

Use DP-TSP when

- Cities ≤ 20
- Exact answer required
- Cost matrix available
- Every city must be visited once

---

Do NOT use DP-TSP when

- Hundreds or thousands of cities
- Approximate solution acceptable
- Real-time response needed

---

# 3. Where is DP-TSP Used?

## Logistics

```
Warehouse

↓

Customer 1

↓

Customer 2

↓

Customer 3

↓

Warehouse
```

---

## Courier Delivery

```
Delivery Vehicle

↓

Shortest Route

↓

Less Fuel

↓

Higher Profit
```

---

## Robotics

Robot visits:

```
Room 1

↓

Room 2

↓

Room 3

↓

Charging Station
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

Minimum Tool Movement
```

---

## Satellite Path Planning

```
Satellite

↓

Target A

↓

Target B

↓

Target C

↓

Return
```

---

## DNA Sequencing

Genome comparison paths often use DP concepts similar to TSP optimization.

---

# 4. How to Solve TSP Using Dynamic Programming

Suppose

```
Cities

0
1
2
3
```

Start from city 0.

---

## Step 1

Represent visited cities using Bitmask.

Example

```
0001

Only City 0 visited
```

```
0101

Cities 0 and 2 visited
```

---

## Step 2

State Definition

```
dp[mask][city]

Minimum cost

to visit cities in mask

ending at city
```

---

## Step 3

Transition

```
Current State

(mask, city)

↓

Visit Next City

↓

(mask | next, next)
```

Formula

```
dp[mask][u] =
min(
cost[u][v] +
dp[mask | (1<<v)][v]
)
```

---

## Step 4

Base Case

All cities visited

```
mask == (1<<N)-1
```

Return

```
cost[current][0]
```

Return to starting city.

---

## Step 5

Answer

```
dp[1][0]
```

---

# 5. DP-TSP Algorithm Explanation

## Recursive View

```
Start

↓

Visit Unvisited City

↓

Repeat

↓

All Cities Visited

↓

Return Home
```

---

## State Transition Diagram

```
(mask,current)

↓

Choose Next

↓

(mask + city,current')

↓

Store Result

↓

Reuse Later
```

---

## DP Table

Example

| Mask | Current | Cost |
|------|---------|------|
|0001|0|0|
|0011|1|10|
|0101|2|15|
|0111|2|25|
|1111|3|35|

---

## Decision Tree

Without DP

```
          A
      /   |   \
     B    C    D
   / | \
  C  D  C
 / \
D   D
```

Many repeated computations.

---

With DP

```
A

↓

Store Every State Once

↓

Reuse

↓

Fast
```

---

# 6. Visual Demonstrations

## Example Graph

```
      (10)
   A--------B
   |\      /|
 15| \35  /25
   |  \  /
   |30\/20
   |  /\
   | /  \
   C------D
     (30)
```

---

## Bitmask Illustration

```
Cities

0
1
2
3

Mask

0001

↓

0011

↓

0111

↓

1111
```

---

## Flowchart

```text
Start
   │
   ▼
Read Cost Matrix
   │
   ▼
Initialize DP Table
   │
   ▼
Recursive Function
   │
   ▼
All Cities Visited?
 ┌─────┴─────┐
 │           │
Yes          No
 │           │
 ▼           ▼
Return      Try Every
Home        Unvisited City
 │           │
 └─────┬─────┘
       ▼
Store Minimum
       │
       ▼
Answer
```

---

## State Expansion

```
(0001,0)

↓

(0011,1)

↓

(0111,2)

↓

(1111,3)

↓

Return to 0
```

---

## Memoization Table

```
State

(0011,1)

↓

Solved

↓

Saved

↓

Reuse Later
```

---

# 7. Limitations and Edge Cases

# Limitation 1 — Exponential States

States

```
2^N × N
```

Visual

```
Cities

4

States

16

Cities

10

States

1024

Cities

20

States

1,048,576

Huge Growth
```

---

# Limitation 2 — High Memory

```
DP Table

N=20

↓

20 × 2^20

↓

Millions of Entries
```

---

# Limitation 3 — NP-Hard

No polynomial-time exact algorithm is known.

```
Cities

↓

Possible Tours

↓

Factorial Growth

↓

Brute Force Impossible

↓

DP Improves

↓

Still Exponential
```

---

# Limitation 4 — Large Inputs

```
10 Cities

Easy

↓

20 Cities

Manageable

↓

30 Cities

Very Slow

↓

100 Cities

Impossible
```

---

# Edge Case 1

Single City

```
A

↓

A

Cost = 0
```

---

# Edge Case 2

Disconnected Graph

```
A ---- B

C

No Path

Answer

Impossible
```

---

# Edge Case 3

Equal Costs

```
10

10

10

Many Optimal Tours
```

---

# Edge Case 4

Negative Costs

```
Generally Not Allowed

Distances

Must be Positive
```

---

# 8. Java Implementation

```java
import java.util.Arrays;

public class TSPDynamicProgramming {

    static final int INF = 1000000000;

    static int[][] cost = {
            {0, 10, 15, 20},
            {10, 0, 35, 25},
            {15, 35, 0, 30},
            {20, 25, 30, 0}
    };

    static int N = cost.length;
    static int[][] dp;

    static int tsp(int mask, int pos) {

        if (mask == (1 << N) - 1) {
            return cost[pos][0];
        }

        if (dp[mask][pos] != -1)
            return dp[mask][pos];

        int ans = INF;

        for (int city = 0; city < N; city++) {

            if ((mask & (1 << city)) == 0) {

                int newCost =
                        cost[pos][city]
                        + tsp(mask | (1 << city), city);

                ans = Math.min(ans, newCost);
            }
        }

        return dp[mask][pos] = ans;
    }

    public static void main(String[] args) {

        dp = new int[1 << N][N];

        for (int[] row : dp)
            Arrays.fill(row, -1);

        int answer = tsp(1, 0);

        System.out.println("Minimum Tour Cost = " + answer);
    }
}
```

---

## Sample Input

```text
Cost Matrix

0 10 15 20
10 0 35 25
15 35 0 30
20 25 30 0
```

---

## Output

```text
Minimum Tour Cost = 80
```

Optimal Tour

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

Cost

10 + 25 + 30 + 15

=

80
```

---

# 9. Complexity Analysis

| Metric | Complexity |
|---------|------------|
|States|O(N·2ᴺ)|
|Transitions|O(N)|
|Time|O(N²·2ᴺ)|
|Space|O(N·2ᴺ)|

---

## Comparison

| Method | Time Complexity |
|---------|----------------|
|Brute Force|O(N!)|
|Dynamic Programming|O(N²·2ᴺ)|

DP is exponentially faster than brute force but still exponential.

---

# 10. Advantages and Disadvantages

## Advantages

- Finds exact optimal solution
- Much faster than brute force
- Eliminates repeated computation
- Elegant use of memoization
- Suitable for small and medium-sized instances

---

## Disadvantages

- Still exponential
- High memory usage
- Unsuitable for large numbers of cities
- Requires complete cost matrix
- Implementation is more complex than greedy methods

---

# 11. Frequently Asked Exam Questions

### Q1. What is Dynamic Programming?

A method that solves overlapping subproblems once and stores their results for reuse.

---

### Q2. What is the Traveling Salesman Problem?

Find the minimum-cost Hamiltonian cycle that visits every city exactly once and returns to the starting city.

---

### Q3. Why is TSP solved using DP?

Because TSP exhibits:

- Optimal Substructure
- Overlapping Subproblems

---

### Q4. State Representation?

```
dp[mask][current]
```

---

### Q5. Time Complexity?

```
O(N² × 2ᴺ)
```

---

### Q6. Space Complexity?

```
O(N × 2ᴺ)
```

---

### Q7. Is DP-TSP polynomial?

No.

It is exponential but significantly better than brute-force enumeration.

---

### Q8. Why use Bitmasking?

To efficiently represent the set of visited cities using binary digits, enabling compact state representation and fast membership checks.

---

# 12. Key Points to Remember

- Dynamic Programming stores solutions to overlapping subproblems.
- TSP asks for the minimum-cost tour visiting each city exactly once.
- DP-TSP state: `dp[mask][current]`.
- Bitmasking efficiently represents visited cities.
- Transition:
  ```
  dp[mask][u] =
  min(cost[u][v] + dp[mask | (1<<v)][v])
  ```
- Base case: all cities visited → return to the starting city.
- Time complexity: **O(N² × 2ᴺ)**.
- Space complexity: **O(N × 2ᴺ)**.
- DP produces an **optimal** solution because every state considers all valid next choices while reusing optimal subproblem solutions.
- Practical only for small-to-medium numbers of cities (typically up to around 20–22 depending on memory and hardware).
- For very large TSP instances, approximation and heuristic algorithms (e.g., nearest neighbor, genetic algorithms, simulated annealing, ant colony optimization) are preferred.
