# Maximum Matching in Bipartite Graphs (Iterative Improvement) — Complete Study Guide

> **Subject:** Design and Analysis of Algorithms (DAA)
>
> **Topic:** Maximum Matching in Bipartite Graphs — Iterative Improvement (Augmenting Path Method)

---

# Table of Contents

- [1. Why This Topic Matters](#1-why-this-topic-matters)
- [2. When and Where It's Used](#2-when-and-where-its-used)
- [3. Understanding Bipartite Graphs](#3-understanding-bipartite-graphs)
- [4. Matching Basics](#4-matching-basics)
- [5. How to Find and Use Maximum Matching](#5-how-to-find-and-use-maximum-matching)
- [6. Visual Explanation](#6-visual-explanation)
- [7. Iterative Improvement (Augmenting Path Algorithm)](#7-iterative-improvement-augmenting-path-algorithm)
- [8. Step-by-Step Example](#8-step-by-step-example)
- [9. Java Implementation](#9-java-implementation)
- [10. Complexity Analysis](#10-complexity-analysis)
- [11. Limitations and Edge Cases](#11-limitations-and-edge-cases)
- [12. Advantages and Disadvantages](#12-advantages-and-disadvantages)
- [13. Exam Tips](#13-exam-tips)
- [14. Frequently Asked Questions](#14-frequently-asked-questions)
- [15. Key Takeaways](#15-key-takeaways)

---

# 1. Why This Topic Matters

Maximum Matching in Bipartite Graphs is one of the most important optimization problems in computer science.

It solves problems where one set of objects must be matched with another set **without conflicts**.

Examples include:

- Assigning employees to jobs
- Assigning students to projects
- Matching hospitals with doctors
- Matching advertisements with users
- Scheduling classrooms
- Resource allocation
- Network routing

It is also the foundation for:

- Network Flow
- Assignment Problems
- Scheduling Algorithms
- Recommendation Systems
- Compiler Register Allocation

---

## Why Engineers Study It

It teaches:

- Graph Modeling
- Optimization
- Greedy Improvement
- Iterative Algorithms
- Augmenting Paths
- Network Flow Concepts

---

# 2. When and Where It's Used

## Real World Applications

| Domain | Application |
|---------|-------------|
| HR Systems | Employee → Job Assignment |
| Universities | Student → Project Allocation |
| Hospitals | Doctor → Shift Assignment |
| Ride Sharing | Driver → Passenger Matching |
| Dating Apps | User Matching |
| Sports | Team Scheduling |
| Cloud Computing | Server → Task Allocation |
| E-commerce | Seller → Buyer Matching |
| Recommendation Systems | User → Product Matching |

---

## Industry Examples

Google

- Ad allocation

Amazon

- Warehouse task assignment

Uber

- Driver allocation

Microsoft

- Resource scheduling

Facebook

- Friend recommendations

Hospitals

- Doctor scheduling

---

# 3. Understanding Bipartite Graphs

A graph is **bipartite** if vertices can be divided into two disjoint sets:

- Left Set (U)
- Right Set (V)

Edges exist **only between the two sets**.

Never:

- Left → Left
- Right → Right

---

## Example

```
Left           Right

A ------------- 1

A ------------- 2

B ------------- 2

B ------------- 3

C ------------- 3
```

---

# 4. Matching Basics

## Matching

A matching is a set of edges where **no two edges share a vertex**.

Example

```
A --- 1

B --- 2

C --- 3
```

Valid.

---

Example

```
A ---1

A ---2
```

Invalid.

A is matched twice.

---

## Maximum Matching

A matching having the **largest possible number of edges**.

Example

```
A → 1

B → 2

C → 3
```

Matching Size = 3

Cannot improve.

Hence maximum.

---

## Perfect Matching

Every vertex is matched.

Example

```
A→1

B→2

C→3
```

All vertices matched.

Perfect Matching.

---

# 5. How to Find and Use Maximum Matching

## Input

- Bipartite graph
- Left vertices
- Right vertices
- Edge list

---

## Output

Maximum number of matched pairs.

Example

Input

```
Workers

A
B
C

Jobs

1
2
3

Edges

A→1
A→2
B→2
C→3
```

Output

```
A→1

B→2

C→3

Maximum Matching = 3
```

---

## General Strategy

1. Start with no matching.
2. Pick an unmatched vertex.
3. Search for an augmenting path.
4. Reverse matched/unmatched edges.
5. Increase matching size.
6. Repeat until no augmenting path exists.

---

# 6. Visual Explanation

## Initial Graph

```
Left             Right

A ------------- 1

A ------------- 2

B ------------- 2

B ------------- 3

C ------------- 3
```

No matching.

---

## Step 1

Choose

```
A → 1
```

Current Matching

```
A ===== 1

B

C
```

("=" denotes matched edge)

---

## Step 2

Choose

```
B → 2
```

```
A ====1

B ====2

C
```

---

## Step 3

Choose

```
C →3
```

```
A ====1

B ====2

C ====3
```

Maximum Matching = 3

---

## Another Example

```
Left             Right

A --------1

A --------2

B --------1

B --------3

C --------2
```

Start

```
A→1
```

Current

```
A====1

B

C
```

Need match for B.

Direct edge:

```
B→1
```

Already occupied.

Look for augmenting path.

```
B

↓

1

↓

A

↓

2
```

Flip

Old

```
A→1
```

New

```
A→2

B→1
```

Matching increased.

---

## Augmenting Path Visualization

Before

```
A ====1

B

2
```

Path

```
B

↓

1

↓

A

↓

2
```

Flip

After

```
B====1

A====2
```

Matching size increased.

---

# 7. Iterative Improvement (Augmenting Path Algorithm)

## Idea

The algorithm repeatedly searches for an **augmenting path**.

An augmenting path:

- Starts at an unmatched left vertex.
- Ends at an unmatched right vertex.
- Alternates between unmatched and matched edges.

---

## Why It Works

Every augmenting path increases matching size by exactly **1**.

No augmenting path means:

**Current matching is maximum.**

This is known as **Berge's Theorem**.

---

## Algorithm

```
Matching = Empty

Repeat

    Find augmenting path

    If found

        Flip edges

    Else

        Stop

Return Matching
```

---

## Flowchart

```
Start

   │

Empty Matching

   │

Find Augmenting Path

   │

Found?

 ┌───────┐
 │ Yes   │
 └───┬───┘
     │

Flip Matching

     │

Repeat

     │

No

     │

Return Maximum Matching
```

---

# 8. Step-by-Step Example

Graph

```
Workers

A
B
C

Jobs

1
2
3

Edges

A→1
A→2
B→2
C→3
```

---

Iteration 1

```
Matching

A→1
```

Size =1

---

Iteration 2

```
Matching

A→1

B→2
```

Size =2

---

Iteration 3

```
Matching

A→1

B→2

C→3
```

Size =3

No augmenting path.

Algorithm stops.

---

# 9. Java Implementation

```java
import java.util.ArrayList;
import java.util.List;

public class MaximumBipartiteMatching {

    private final int leftVertices;
    private final int rightVertices;
    private final List<Integer>[] graph;

    @SuppressWarnings("unchecked")
    public MaximumBipartiteMatching(int leftVertices, int rightVertices) {
        this.leftVertices = leftVertices;
        this.rightVertices = rightVertices;

        graph = new ArrayList[leftVertices];

        for (int i = 0; i < leftVertices; i++) {
            graph[i] = new ArrayList<>();
        }
    }

    // Add an edge from left vertex to right vertex
    public void addEdge(int left, int right) {
        graph[left].add(right);
    }

    // DFS to find augmenting path
    private boolean findMatch(int left,
                              boolean[] visited,
                              int[] matchRight) {

        for (int right : graph[left]) {

            if (visited[right])
                continue;

            visited[right] = true;

            // Right node is free
            if (matchRight[right] == -1 ||
                    findMatch(matchRight[right],
                              visited,
                              matchRight)) {

                matchRight[right] = left;
                return true;
            }
        }

        return false;
    }

    public int maximumMatching() {

        int[] matchRight = new int[rightVertices];

        for (int i = 0; i < rightVertices; i++)
            matchRight[i] = -1;

        int result = 0;

        for (int left = 0; left < leftVertices; left++) {

            boolean[] visited = new boolean[rightVertices];

            if (findMatch(left, visited, matchRight))
                result++;
        }

        System.out.println("Matched Pairs:");

        for (int i = 0; i < rightVertices; i++) {

            if (matchRight[i] != -1) {

                System.out.println(
                        "Left " + matchRight[i]
                                + " -> Right " + i);
            }
        }

        return result;
    }

    public static void main(String[] args) {

        MaximumBipartiteMatching graph =
                new MaximumBipartiteMatching(3, 3);

        graph.addEdge(0, 0);
        graph.addEdge(0, 1);
        graph.addEdge(1, 1);
        graph.addEdge(1, 2);
        graph.addEdge(2, 2);

        int max = graph.maximumMatching();

        System.out.println(
                "\nMaximum Matching = " + max);
    }
}
```

---

## Sample Output

```
Matched Pairs:

Left 0 -> Right 0

Left 1 -> Right 1

Left 2 -> Right 2

Maximum Matching = 3
```

---

# 10. Complexity Analysis

## Time Complexity

For the DFS-based augmenting path algorithm:

```
O(V × E)
```

where:

- **V** = number of vertices
- **E** = number of edges

Reason:

- DFS may be performed once for each left-side vertex.
- Each DFS may explore many edges.

---

## Space Complexity

```
O(V + E)
```

Used for:

- Graph representation
- Matching array
- Visited array
- DFS recursion stack

---

## Comparison

| Algorithm | Time Complexity |
|------------|----------------|
| DFS Augmenting Path | O(VE) |
| Hopcroft-Karp | O(E√V) |
| Hungarian Algorithm (Weighted) | O(n³) |

---

# 11. Limitations and Edge Cases

## 1. Disconnected Graph

```
A ----1

B

C----2
```

Matching

```
A→1

C→2
```

Disconnected components are processed independently.

---

## 2. No Perfect Matching

```
Left

A

B

C

Right

1

2
```

Maximum

```
A→1

B→2
```

C remains unmatched.

---

## 3. Unbalanced Graph

```
Left

A
B
C
D
E

Right

1
2
```

Maximum Matching

```
2
```

Only two right-side vertices exist.

---

## 4. Empty Graph

```
No vertices

No edges
```

Output

```
Maximum Matching = 0
```

---

## 5. Single Node

```
A ----1
```

Matching

```
A→1
```

Size =1

---

## 6. Isolated Vertices

```
A

B----1

C
```

Only

```
B→1
```

Possible.

---

## 7. Multiple Choices

```
A→1

A→2

B→1

B→2
```

Several optimal solutions exist.

Possible Solution

```
A→1

B→2
```

or

```
A→2

B→1
```

Both are maximum.

---

# 12. Advantages and Disadvantages

## Advantages

- Simple implementation
- Guarantees optimal matching
- Widely used
- Easy to understand
- Works for sparse graphs
- Foundation for advanced algorithms

---

## Disadvantages

- DFS version is slower on large graphs
- Not suitable for weighted assignment problems
- Recursive DFS may cause deep recursion on very large graphs
- Faster algorithms exist (Hopcroft-Karp)

---

# 13. Exam Tips

Remember these definitions:

### Bipartite Graph

Vertices divided into two independent sets.

---

### Matching

No vertex appears in more than one selected edge.

---

### Maximum Matching

Largest possible matching.

---

### Perfect Matching

Every vertex is matched.

---

### Augmenting Path

Alternating path that:

- Starts unmatched
- Ends unmatched
- Alternates between unmatched and matched edges

Flipping the path increases matching size by **1**.

---

### Iterative Improvement

Repeatedly:

- Find augmenting path
- Flip edges
- Increase matching
- Stop when no augmenting path exists

---

# 14. Frequently Asked Questions

## Q1. Is every maximum matching perfect?

No.

Perfect matching is only possible if every vertex can be matched.

---

## Q2. Can multiple maximum matchings exist?

Yes.

Different edge selections may produce the same maximum size.

---

## Q3. What proves optimality?

**Berge's Theorem**:

> A matching is maximum if and only if there is no augmenting path.

---

## Q4. Which algorithm is fastest?

- DFS Augmenting Path: Good for learning and moderate-sized graphs.
- Hopcroft-Karp: Best for large unweighted bipartite graphs.
- Hungarian Algorithm: Used for weighted assignment problems.

---

# 15. Key Takeaways

> **Exam Summary**

- Bipartite graphs divide vertices into two independent sets.
- A **matching** contains edges that do not share endpoints.
- A **maximum matching** has the largest number of matched pairs.
- A **perfect matching** matches every vertex.
- The iterative improvement approach repeatedly finds **augmenting paths** and flips matched/unmatched edges.
- Each augmenting path increases the matching size by exactly **1**.
- When no augmenting path exists, the current matching is maximum (Berge's Theorem).
- The DFS-based augmenting path algorithm runs in **O(V × E)** time and **O(V + E)** space.
- For very large graphs, **Hopcroft-Karp** provides a more efficient solution with **O(E√V)** complexity.
