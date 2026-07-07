# Warshall and Floyd Algorithms – Complete Study Guide

## Table of Contents

- [1. Introduction](#1-introduction)
- [2. Why, When, and Where](#2-why-when-and-where)
- [3. Warshall vs Floyd](#3-warshall-vs-floyd)
- [4. Warshall Algorithm](#4-warshall-algorithm)
- [5. Floyd Algorithm](#5-floyd-algorithm)
- [6. Step-by-Step Visual Walkthroughs](#6-step-by-step-visual-walkthroughs)
- [7. Complexity Analysis](#7-complexity-analysis)
- [8. Correctness](#8-correctness)
- [9. Limitations and Edge Cases](#9-limitations-and-edge-cases)
- [10. Java Implementations](#10-java-implementations)
- [11. Exam Tips](#11-exam-tips)

# 1. Introduction

Both algorithms use **Dynamic Programming** and operate on an adjacency matrix.

| Algorithm | Solves |
|-----------|--------|
| Warshall | Transitive Closure (Reachability) |
| Floyd | All-Pairs Shortest Path |

---

# 2. Why, When, and Where

## Warshall

Use when the question asks:

- Can vertex A reach B?
- Connectivity between every pair
- Transitive closure

Applications:

- Social networks
- Dependency graphs
- Compiler analysis
- Database query optimization

## Floyd

Use when the question asks:

- Shortest distance between every pair
- Routing tables
- Road networks
- Network optimization

---

# 3. Warshall vs Floyd

| Feature | Warshall | Floyd |
|---------|----------|--------|
| Output | Reachability | Shortest distance |
| Matrix | Boolean | Integer/Weight |
| Negative edges | Not applicable | Allowed |
| Negative cycle | No concept | Detectable |
| Complexity | O(V³) | O(V³) |
| Space | O(V²) | O(V²) |

---

# 4. Warshall Algorithm

## Idea

If path i→k and k→j exist, then path i→j exists.

Formula

```text
R[i][j] = R[i][j] OR (R[i][k] AND R[k][j])
```

Algorithm

1. Start with adjacency matrix.
2. Choose each vertex as intermediate.
3. Update reachability.
4. Final matrix is transitive closure.

ASCII

```text
i ----> k ----> j

If both exist

i -----------> j
```

---

# 5. Floyd Algorithm

## Idea

Try every vertex as an intermediate.

Formula

```text
dist[i][j] =
min(
dist[i][j],
dist[i][k] + dist[k][j]
)
```

Algorithm

1. Initialize distance matrix.
2. Put INF where edge absent.
3. For each k
4. Relax all i,j pairs.
5. Final matrix contains shortest paths.

---

# 6. Step-by-step Visual Walkthroughs

## Warshall Example

Adjacency Matrix

| |A|B|C|
|-|-|-|-|
|A|1|1|0|
|B|0|1|1|
|C|0|0|1|

After considering B

Since

```text
A → B
B → C
```

Then

```text
A → C
```

New matrix

| |A|B|C|
|-|-|-|-|
|A|1|1|1|
|B|0|1|1|
|C|0|0|1|

---

## Floyd Example

Graph

```text
A --3--> B --1--> C
 \       ^
  \10    |
    -----
```

Initial

| |A|B|C|
|-|-|-|-|
|A|0|3|10|
|B|INF|0|1|
|C|INF|INF|0|

Using B

A→C

```text
3 + 1 = 4
```

Updated

| |A|B|C|
|-|-|-|-|
|A|0|3|4|
|B|INF|0|1|
|C|INF|INF|0|

---

# 7. Complexity Analysis

| Algorithm | Time | Space |
|-----------|------|--------|
| Warshall | O(V³) | O(V²) |
| Floyd | O(V³) | O(V²) |

---

# 8. Correctness

## Warshall

After iteration k, paths using vertices 1..k as intermediates are correctly represented.

## Floyd

After iteration k, shortest paths using vertices 1..k as intermediate vertices have been computed.

---

# 9. Limitations and Edge Cases

## Disconnected Graph

```text
A    B
```

Warshall

No path.

Floyd

Distance remains INF.

---

## Self Loop

```text
A → A
```

Diagonal remains reachable.

Distance stays 0.

---

## Negative Edge

```text
A --(-2)--> B
```

Floyd works correctly if no negative cycle.

---

## Negative Cycle

```text
A → B (-3)

B → A (-2)
```

Detection

```text
dist[i][i] < 0
```

If true

Negative cycle exists.

---

Better Alternatives

| Situation | Better Algorithm |
|-----------|------------------|
| Single source positive weights | Dijkstra |
| Single source negative edges | Bellman-Ford |
| Sparse graph APSP | Repeated Dijkstra |

---

# 10. Java Implementations

## Warshall

```java
public class Warshall {

    static void warshall(int[][] graph) {
        int n = graph.length;
        int[][] reach = new int[n][n];

        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                reach[i][j] = graph[i][j];

        for (int k = 0; k < n; k++)
            for (int i = 0; i < n; i++)
                for (int j = 0; j < n; j++)
                    reach[i][j] =
                        (reach[i][j] == 1 ||
                        (reach[i][k] == 1 && reach[k][j] == 1)) ? 1 : 0;

        System.out.println("Transitive Closure");

        for (int[] row : reach) {
            for (int x : row)
                System.out.print(x + " ");
            System.out.println();
        }
    }

    public static void main(String[] args) {

        int[][] graph = {
            {1,1,0},
            {0,1,1},
            {0,0,1}
        };

        warshall(graph);
    }
}
```

Output

```text
1 1 1
0 1 1
0 0 1
```

---

## Floyd

```java
public class Floyd {

    static final int INF = 99999;

    static void floyd(int[][] dist) {

        int V = dist.length;

        for (int k = 0; k < V; k++)
            for (int i = 0; i < V; i++)
                for (int j = 0; j < V; j++)
                    if (dist[i][k] != INF &&
                        dist[k][j] != INF &&
                        dist[i][k] + dist[k][j] < dist[i][j])
                        dist[i][j] = dist[i][k] + dist[k][j];

        for (int i = 0; i < V; i++)
            if (dist[i][i] < 0) {
                System.out.println("Negative Cycle Detected");
                return;
            }

        System.out.println("Shortest Distance Matrix");

        for (int[] row : dist) {
            for (int x : row)
                System.out.print((x == INF ? "INF" : x) + " ");
            System.out.println();
        }
    }

    public static void main(String[] args) {

        int[][] graph = {
            {0,3,10},
            {INF,0,1},
            {INF,INF,0}
        };

        floyd(graph);
    }
}
```

Output

```text
0 3 4
INF 0 1
INF INF 0
```

---

# 11. Exam Tips

- Warshall = Reachability.
- Floyd = Shortest Paths.
- Both use Dynamic Programming.
- Triple nested loops.
- Time Complexity = O(V³).
- Space Complexity = O(V²).
- Floyd detects negative cycles using `dist[i][i] < 0`.
- Dijkstra is usually better for a single source with non-negative weights.
