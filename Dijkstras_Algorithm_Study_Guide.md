
# Dijkstra's Algorithm – Complete Study Guide (Design and Analysis of Algorithms)

## Table of Contents

- [1. Introduction](#1-introduction)
- [2. Why Dijkstra's Algorithm](#2-why-dijkstras-algorithm)
- [3. When and Where to Use](#3-when-and-where-to-use)
- [4. Key Concepts](#4-key-concepts)
- [5. Working Principle](#5-working-principle)
- [6. Step-by-Step Algorithm](#6-step-by-step-algorithm)
- [7. Visual Explanation](#7-visual-explanation)
- [8. Example Walkthrough](#8-example-walkthrough)
- [9. Pseudocode](#9-pseudocode)
- [10. Java Implementation](#10-java-implementation)
- [11. Time and Space Complexity](#11-time-and-space-complexity)
- [12. Limitations and Edge Cases](#12-limitations-and-edge-cases)
- [13. Comparison](#13-comparison)
- [14. Exam Tips](#14-exam-tips)
- [15. Viva Questions](#15-viva-questions)
- [16. Summary](#16-summary)

---

# 1. Introduction

**Dijkstra's Algorithm** is a **Greedy Algorithm** used to find the **shortest path from one source vertex to every other vertex** in a weighted graph **having non-negative edge weights**.

It repeatedly chooses the nearest unvisited vertex and relaxes its neighboring edges.

---

# 2. Why Dijkstra's Algorithm

## Problem Solved

Given a weighted graph:

- Source vertex
- Destination vertices
- Positive edge weights

Find the minimum distance from the source.

### Why Needed?

Without Dijkstra:

- Many possible paths
- Exhaustive search is expensive

Dijkstra intelligently expands the cheapest path first.

---

# 3. When and Where to Use

## Use When

- Graph has **non-negative weights**
- Single source shortest path
- Static graph

## Do NOT Use

- Negative edge weights
- Negative cycles

## Applications

- GPS Navigation
- Google Maps
- Network Routing
- Robot Navigation
- Airline Routes
- Game AI
- Packet Routing
- Delivery Optimization

---

# 4. Key Concepts

| Term | Meaning |
|------|---------|
| Source | Starting vertex |
| Distance | Minimum cost from source |
| Relaxation | Updating shorter distance |
| Priority Queue | Picks smallest distance vertex |
| Visited | Finalized shortest path |

---

# 5. Working Principle

Greedy Choice:

Always process the unvisited vertex having the minimum tentative distance.

Relax every outgoing edge.

If a shorter path is found:

```
newDistance = distance[u] + weight(u,v)
```

Update.

---

# 6. Step-by-Step Algorithm

```
Initialize all distances = ∞
distance[source] = 0

Insert source into Priority Queue

While queue not empty

    Extract minimum distance node

    For every neighbor

        Relax edge

            if shorter path found

                update distance

                push into queue
```

---

# 7. Visual Explanation

## Sample Graph

```text
          4
     A -------- B
     | \        |
   2 |  \5      |10
     |   \      |
     C----D-----E
      \1  |2   /
       \  |   /3
         \|  /
          F
```

Source = A

### Initial

|Vertex|Distance|
|------|--------|
|A|0|
|B|∞|
|C|∞|
|D|∞|
|E|∞|
|F|∞|

---

Flow

```text
Start

↓

Source = 0

↓

Choose Minimum Distance Vertex

↓

Relax Adjacent Edges

↓

Update Distances

↓

Repeat Until Queue Empty

↓

Finished
```

---

# 8. Example Walkthrough

Edges

```text
A→B = 4
A→C = 2
A→D = 5
C→F = 1
F→D = 2
D→E = 3
B→E = 10
```

### Iteration 1

Visit A

|Vertex|Distance|
|------|--------|
|A|0|
|B|4|
|C|2|
|D|5|
|E|∞|
|F|∞|

Choose C.

### Iteration 2

Visit C

Update F

Distance(F)=3

### Iteration 3

Visit F

Update D

Distance(D)=5 (unchanged)

### Iteration 4

Visit B

Update E

Distance(E)=14

### Iteration 5

Visit D

Update E

Distance(E)=8

Final

|Vertex|Shortest Distance|
|------|-----------------|
|A|0|
|B|4|
|C|2|
|D|5|
|E|8|
|F|3|

Shortest Path A→E

```text
A
↓

C
↓

F
↓

D
↓

E
```

Cost = 8

---

# 9. Pseudocode

```text
DIJKSTRA(G, source)

for each vertex
    distance = ∞

distance[source]=0

PriorityQueue pq

insert(source)

while pq not empty

    u = extractMin()

    for each neighbor v

        if distance[u]+weight(u,v) < distance[v]

            distance[v]=distance[u]+weight(u,v)

            insert(v)

return distance
```

---

# 10. Java Implementation

```java
import java.util.*;

class Edge {
    int vertex, weight;

    Edge(int vertex, int weight) {
        this.vertex = vertex;
        this.weight = weight;
    }
}

public class Dijkstra {

    static void dijkstra(List<List<Edge>> graph, int source) {

        int V = graph.size();

        int[] dist = new int[V];
        Arrays.fill(dist, Integer.MAX_VALUE);

        PriorityQueue<Edge> pq =
            new PriorityQueue<>(Comparator.comparingInt(e -> e.weight));

        dist[source] = 0;
        pq.offer(new Edge(source, 0));

        while (!pq.isEmpty()) {

            Edge current = pq.poll();

            int u = current.vertex;

            if (current.weight > dist[u])
                continue;

            for (Edge edge : graph.get(u)) {

                int v = edge.vertex;
                int weight = edge.weight;

                if (dist[u] + weight < dist[v]) {

                    dist[v] = dist[u] + weight;

                    pq.offer(new Edge(v, dist[v]));
                }
            }
        }

        System.out.println("Shortest distances:");

        for (int i = 0; i < V; i++)
            System.out.println(source + " -> " + i + " = " + dist[i]);
    }
}
```

---

# 11. Time and Space Complexity

|Implementation|Time|
|-------------|----|
|Adjacency Matrix|O(V²)|
|Adjacency List + Binary Heap|O((V+E)logV)|
|Fibonacci Heap|O(E + VlogV)|

Space = O(V+E)

---

# 12. Limitations and Edge Cases

## 1. Negative Edge

```text
A ----2---- B

 \

 -5

  \

   C
```

Fails because greedy choice becomes invalid.

Use **Bellman-Ford** instead.

---

## 2. Negative Cycle

```text
A → B → C

↑       ↓

← -4 ←
```

Shortest path is undefined.

---

## 3. Disconnected Graph

```text
A ----- B


C ----- D
```

Source=A

Distance(C)=∞

Distance(D)=∞

---

## 4. Multiple Equal Paths

```text
A

/ \

2   2

B   C

 \ /

 2

 D
```

Either shortest path is acceptable.

---

## 5. Single Vertex

```text
A
```

Distance=0

---

## 6. Self Loop

```text
A

↺3
```

Normally ignored unless beneficial.

---

## 7. Large Graphs

Priority Queue greatly improves performance.

---

# 13. Comparison

|Algorithm|Negative Edge|All Pairs|Greedy|
|----------|-------------|----------|-------|
|Dijkstra|No|No|Yes|
|Bellman-Ford|Yes|No|No|
|Floyd-Warshall|Yes|Yes|No|

---

# 14. Exam Tips

Remember:

- Greedy algorithm
- Single source shortest path
- Non-negative edges only
- Relaxation updates distances
- Priority Queue gives efficiency
- Complexity:
    - Matrix → O(V²)
    - Heap → O((V+E)logV)

Common exam questions:

1. Write algorithm.
2. Dry run.
3. Trace table.
4. Complexity.
5. Difference from Bellman-Ford.

---

# 15. Viva Questions

1. Why can't Dijkstra handle negative edges?
2. What is relaxation?
3. Why Priority Queue?
4. What is greedy choice?
5. Difference between visited and unvisited?
6. Can multiple shortest paths exist?
7. Complexity with adjacency list?
8. Which algorithm handles negative weights?
9. What happens for disconnected graphs?
10. Give two real-world applications.

---

# 16. Summary

- Greedy shortest-path algorithm.
- Computes shortest distance from one source to every reachable vertex.
- Requires **non-negative edge weights**.
- Efficient with adjacency lists and a priority queue.
- Widely used in routing, navigation, and networking.
