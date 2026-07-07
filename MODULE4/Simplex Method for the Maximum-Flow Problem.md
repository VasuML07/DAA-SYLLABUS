# Design and Analysis of Algorithms (DAA)
# Simplex Method, Maximum-Flow Problem & Iterative Improvement
### Complete Exam Study Guide

> **Semester Exam Notes**
>
> Topics Covered:
>
> - Simplex Method (Linear Programming)
> - Maximum-Flow Problem
> - Iterative Improvement
> - Relationship between these optimization techniques

---

# Table of Contents

- [1. Why These Topics Matter](#1-why-these-topics-matter)
- [2. When & Where These Are Used](#2-when--where-these-are-used)
- [3. Relationship Between the Topics](#3-relationship-between-the-topics)
- [4. How to Solve Optimization Problems](#4-how-to-solve-optimization-problems)
- [5. Simplex Method](#5-simplex-method)
- [6. Maximum-Flow Problem](#6-maximum-flow-problem)
- [7. Iterative Improvement](#7-iterative-improvement)
- [8. Visual Explanations](#8-visual-explanations)
- [9. Java Implementations](#9-java-implementations)
- [10. Worked Examples](#10-worked-examples)
- [11. Complexity Analysis](#11-complexity-analysis)
- [12. Limitations & Edge Cases](#12-limitations--edge-cases)
- [13. Comparison Table](#13-comparison-table)
- [14. Exam Tips](#14-exam-tips)
- [15. Key Takeaways](#15-key-takeaways)

---

# 1. Why These Topics Matter

Optimization is one of the most important areas of computer science.

Many real-world problems ask:

- How can we maximize profit?
- How can we minimize cost?
- How can we allocate limited resources?
- How can we transport maximum goods?
- How can we schedule work efficiently?

Three important optimization techniques are:

- **Simplex Method**
- **Maximum Flow**
- **Iterative Improvement**

Together they form the foundation of operations research, optimization theory, AI, networking, logistics, transportation, and economics.

---

## Why Engineers Learn Them

These algorithms teach how to:

- Optimize systems
- Improve solutions step by step
- Solve constrained problems
- Design efficient algorithms
- Model real-world situations mathematically

---

# 2. When & Where These Are Used

## Applications of Simplex Method

- Manufacturing planning
- Production optimization
- Budget allocation
- Diet optimization
- Airline scheduling
- Investment planning

---

## Applications of Maximum Flow

- Internet routing
- Water pipelines
- Road traffic
- Electrical grids
- Supply chain
- Image segmentation
- Network security

---

## Applications of Iterative Improvement

- Machine Learning
- Neural Networks
- Local Search
- Hill Climbing
- Simulated Annealing
- Maximum Matching
- Simplex Method itself

---

## Industry Examples

| Industry | Application |
|-----------|-------------|
| Amazon | Delivery optimization |
| Google | Network routing |
| Uber | Driver assignment |
| Airlines | Crew scheduling |
| Hospitals | Staff scheduling |
| Telecom | Bandwidth allocation |
| Banking | Portfolio optimization |

---

# 3. Relationship Between the Topics

```
              Optimization

                    │

      ┌─────────────┼─────────────┐

      │             │             │

  Simplex      Maximum Flow   Iterative

   Method                       Improvement

      │             │             │

Linear Program   Network     Improve Current

                  Flow          Solution

      │             │             │

      └─────────────┼─────────────┘

            Better Optimal Solution
```

---

## Common Goal

All three algorithms attempt to improve a solution until the best possible solution is found.

---

# 4. How to Solve Optimization Problems

## Step 1

Identify

- Variables
- Constraints
- Objective

---

## Step 2

Model problem

Example

```
Maximize Profit

Subject to

Resource Constraints
```

---

## Step 3

Choose algorithm

| Problem | Algorithm |
|-----------|-----------|
| Linear Optimization | Simplex |
| Network Routing | Maximum Flow |
| General Optimization | Iterative Improvement |

---

## Step 4

Improve solution repeatedly

Continue until

```
No Better Solution Exists
```

---

# 5. Simplex Method

---

## Definition

Simplex Method is an algorithm used to solve **Linear Programming (LP)** problems.

It moves from one feasible solution to another until the optimum solution is reached.

---

## Linear Programming Form

```
Maximize

Z = c1x1 + c2x2 + ...

Subject To

Ax ≤ b

x ≥ 0
```

---

## Important Terms

### Objective Function

Function to maximize or minimize.

Example

```
Maximize

Z = 3x + 5y
```

---

### Constraints

Restrictions.

Example

```
x + y ≤ 4

2x + y ≤ 5
```

---

### Feasible Region

Set of all valid solutions.

---

### Basic Feasible Solution (BFS)

A corner point of feasible region.

Simplex moves from one BFS to another.

---

## Working Principle

```
Initial Tableau

↓

Choose Entering Variable

↓

Choose Leaving Variable

↓

Pivot

↓

New Tableau

↓

Repeat

↓

Optimal Solution
```

---

## Pivot Operation

The pivot transforms the tableau into another equivalent tableau with an improved objective value.

---

# 6. Maximum-Flow Problem

---

## Definition

Given

- Source (S)
- Sink (T)
- Capacities

Find

```
Maximum Flow
```

from Source to Sink.

---

## Example Network

```text
          10
     S -------- A
     |          |
   5 |          |15
     |          |
     B -------- T
          10
```

---

## Terms

### Capacity

Maximum possible flow.

### Flow

Actual flow.

### Residual Capacity

Remaining unused capacity.

---

## Ford-Fulkerson Algorithm

1. Find augmenting path.
2. Find bottleneck capacity.
3. Send flow.
4. Update residual graph.
5. Repeat.

---

## Why It Works

Each iteration increases total flow.

Stops when no augmenting path exists.

---

# 7. Iterative Improvement

---

## Definition

Start with an initial solution.

Improve it repeatedly until

```
No Better Solution Exists.
```

---

## Generic Algorithm

```
Current Solution

↓

Generate Neighbor

↓

Better?

↓

Yes

↓

Replace Current

↓

Repeat

↓

No

↓

Stop
```

---

## Used In

- Simplex
- Maximum Matching
- Hill Climbing
- Local Search
- Simulated Annealing
- Gradient Descent

---

# 8. Visual Explanations

---

# 8.1 Simplex Tableau

Initial

```
Max Z = 3x + 2y

Subject to

x+y≤4

2x+y≤5
```

Tableau

```
+-------------------------------+

|Basis| x | y | s1 | s2 | RHS |

+-------------------------------+

| s1 | 1 | 1 | 1 | 0 | 4 |

| s2 | 2 | 1 | 0 | 1 | 5 |

| Z | -3|-2| 0 | 0 | 0 |

+-------------------------------+
```

---

After Pivot

```
Pivot Element

↓

Basis Changes

↓

Objective Improves
```

```
+-------------------------------+

|Basis| x | y | s1 | s2 | RHS |

+-------------------------------+

| x |1|0|1|-1|1|

| s1|0|1|-2|1|3|

| Z |0|1|3|0|3|

+-------------------------------+
```

---

# 8.2 Maximum Flow

```text
             10

      S ------------ A

      |              |

   5  |              | 15

      |              |

      B ------------ T

            10
```

Flow

```
S→A =10

S→B =5

A→T =10

B→T =5

Maximum Flow =15
```

---

Residual Graph

```
Forward Capacity

↓

Reduced

Backward Edge

↓

Created
```

---

# 8.3 Iterative Improvement

```
Poor Solution

↓

Better

↓

Better

↓

Better

↓

Optimal
```

```
Iteration

1

↓

2

↓

3

↓

4

↓

Stop
```

---

# 8.4 Basis Change

```
Old Basis

{s1,s2}

↓

Pivot

↓

New Basis

{x,s2}
```

---

# 9. Java Implementations

---

# 9.1 Simplex Method (Educational Version)

```java
public class SimplexDemo {

    public static void main(String[] args) {

        System.out.println("Simplex Method");

        System.out.println("1. Create Tableau");

        System.out.println("2. Find Entering Variable");

        System.out.println("3. Find Leaving Variable");

        System.out.println("4. Pivot");

        System.out.println("5. Repeat");

        System.out.println("Optimal Solution Found");
    }

}
```

---

# 9.2 Ford-Fulkerson Maximum Flow

```java
import java.util.*;

public class MaxFlow {

    static final int V = 6;

    boolean bfs(int[][] rGraph, int s, int t, int[] parent) {

        boolean[] visited = new boolean[V];

        Queue<Integer> q = new LinkedList<>();

        q.add(s);

        visited[s] = true;

        parent[s] = -1;

        while (!q.isEmpty()) {

            int u = q.poll();

            for (int v = 0; v < V; v++) {

                if (!visited[v] && rGraph[u][v] > 0) {

                    q.add(v);

                    parent[v] = u;

                    visited[v] = true;
                }
            }
        }

        return visited[t];
    }

    int fordFulkerson(int[][] graph, int s, int t) {

        int u, v;

        int[][] rGraph = new int[V][V];

        for (u = 0; u < V; u++)
            for (v = 0; v < V; v++)
                rGraph[u][v] = graph[u][v];

        int[] parent = new int[V];

        int maxFlow = 0;

        while (bfs(rGraph, s, t, parent)) {

            int pathFlow = Integer.MAX_VALUE;

            for (v = t; v != s; v = parent[v]) {

                u = parent[v];

                pathFlow = Math.min(pathFlow,
                        rGraph[u][v]);
            }

            for (v = t; v != s; v = parent[v]) {

                u = parent[v];

                rGraph[u][v] -= pathFlow;

                rGraph[v][u] += pathFlow;
            }

            maxFlow += pathFlow;
        }

        return maxFlow;
    }
}
```

---

# 9.3 Iterative Improvement Pattern

```java
public class IterativeImprovement {

    public static void main(String[] args) {

        int solution = 10;

        while (true) {

            int better = solution + 1;

            if (better <= solution)

                break;

            solution = better;

            if (solution == 20)

                break;
        }

        System.out.println(solution);
    }
}
```

---

# 10. Worked Examples

---

# Example 1

## Linear Programming

Maximize

```
Z = 3x + 5y
```

Subject To

```
x+y≤4

2x+y≤5
```

Process

```
Create Tableau

↓

Pivot

↓

Pivot

↓

Optimal
```

Result

```
Maximum Profit

Z = 17
```

---

# Example 2

## Maximum Flow

Network

```text
S ----10---- A

|             |

5             15

|             |

B ----10---- T
```

Iteration

```
S→A→T

Flow =10
```

Iteration

```
S→B→T

Flow =5
```

Total

```
15
```

---

# Example 3

## Iterative Improvement

Current

```
Score=50
```

Neighbor

```
60
```

Accept

```
60
```

Next

```
75
```

Accept

Next

```
80
```

Accept

Next

```
79
```

Reject

Stop

Optimal

```
80
```

---

# 11. Complexity Analysis

| Algorithm | Time Complexity | Space |
|------------|----------------|--------|
| Simplex | Exponential (Worst), Polynomial-like in practice | O(mn) |
| Ford-Fulkerson | O(E × MaxFlow) |
| Edmonds-Karp | O(VE²) |
| Iterative Improvement | Depends on neighborhood |

---

# 12. Limitations & Edge Cases

---

## Simplex Degeneracy

```
Two Bases

↓

Same Vertex
```

Can lead to cycling.

---

## Cycling

```
A

↓

B

↓

C

↓

A
```

Never reaches optimum unless anti-cycling rules (e.g., Bland's Rule) are used.

---

## Unbounded Solution

```
Objective

↑

∞
```

No upper bound.

---

## Infeasible Solution

```
Constraint1

×

Constraint2

No Intersection
```

No feasible solution.

---

## Numerical Instability

Large coefficients

```
1000000

0.000001
```

May introduce floating-point errors.

---

## Maximum Flow

### Disconnected Graph

```text
S ----A

B----T
```

Flow

```
0
```

---

### Zero Capacity

```text
S --0--> A
```

Flow

```
0
```

---

### Bottleneck

```text
S--100-->A--1-->T
```

Maximum Flow

```
1
```

---

### Multiple Paths

```text
S

├──5──A──5──T

└──5──B──5──T
```

Maximum Flow

```
10
```

---

## Iterative Improvement

### Local Optimum

```
Global Peak

      ▲

Local ▲

Current
```

Algorithm may stop before reaching the global optimum.

---

### Poor Initial Solution

```
Bad Start

↓

Slow Convergence
```

---

# 13. Comparison Table

| Feature | Simplex | Maximum Flow | Iterative Improvement |
|----------|----------|--------------|-----------------------|
| Problem Type | Linear Programming | Network Optimization | General Optimization |
| Goal | Max/Min Objective | Maximize Flow | Improve Solution |
| Technique | Pivoting | Augmenting Paths | Local Improvement |
| Uses Constraints | Yes | Yes | Depends |
| Optimal Solution | Yes | Yes | Not Always |
| Main Idea | Change Basis | Increase Flow | Improve Current Solution |

---

# 14. Exam Tips

## Remember

### Simplex

- Objective Function
- Constraints
- Feasible Region
- Basic Feasible Solution (BFS)
- Pivot
- Tableau
- Optimality Condition

---

### Maximum Flow

- Source
- Sink
- Capacity
- Flow
- Residual Graph
- Augmenting Path
- Bottleneck Edge

---

### Iterative Improvement

- Initial Solution
- Neighbor Solution
- Improvement
- Stop Condition
- Local Optimum
- Global Optimum

---

### Frequently Asked Theory Questions

1. Define Linear Programming.
2. Explain the Simplex Method.
3. What is a pivot operation?
4. Define Maximum Flow.
5. Explain Ford-Fulkerson.
6. What is an augmenting path?
7. Explain residual graph.
8. What is iterative improvement?
9. Compare Simplex and Maximum Flow.
10. Discuss limitations of iterative improvement.

---

# 15. Key Takeaways

> **Final Revision Sheet**

- Optimization aims to maximize or minimize an objective while satisfying constraints.
- The **Simplex Method** solves linear programming problems by moving between basic feasible solutions using pivot operations.
- The **Maximum-Flow Problem** finds the greatest possible flow from a source to a sink while respecting edge capacities; Ford–Fulkerson increases flow through augmenting paths.
- **Iterative Improvement** is a general optimization strategy that repeatedly improves a current solution until no further improvement is possible.
- Simplex, Maximum Flow, and many other optimization algorithms follow the common principle of progressively improving a solution.
- Understand the key concepts: **objective function, constraints, tableau, pivot, basis, augmenting path, residual graph, bottleneck edge, and local/global optimum**.
- Be familiar with common edge cases such as **cycling, unbounded or infeasible LPs, disconnected flow networks, zero-capacity edges, and local optima**.
- Know the typical complexity:
  - **Simplex:** Exponential worst case, but efficient in practice.
  - **Ford–Fulkerson:** `O(E × MaxFlow)` (or `O(EF)` where `F` is the maximum flow value).
  - **Edmonds–Karp:** `O(VE²)`.
- In exams, practice drawing **tableaux**, **residual graphs**, and **augmenting paths**, and explain each iteration clearly.
