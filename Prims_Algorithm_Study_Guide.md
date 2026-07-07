# Prim's Algorithm -- Design and Analysis of Algorithms

## Table of Contents

-   [Why Prim's Algorithm](#why-prims-algorithm)
-   [When to Use Prim's Algorithm](#when-to-use-prims-algorithm)
-   [Where Prim's Algorithm is Used](#where-prims-algorithm-is-used)
-   [How Prim's Algorithm Works](#how-prims-algorithm-works)
-   [Algorithm Explanation with Visual
    Diagrams](#algorithm-explanation-with-visual-diagrams)
-   [Limitations and Edge Cases](#limitations-and-edge-cases)
-   [Java Implementation](#java-implementation)
-   [Complexity Analysis](#complexity-analysis)
-   [Example Walkthrough](#example-walkthrough)
-   [Exam Tips](#exam-tips)

# Why Prim's Algorithm

Prim's Algorithm finds the **Minimum Spanning Tree (MST)** of a
**connected, weighted, undirected graph**.

An MST: - Connects all vertices. - Contains exactly **V − 1 edges**. -
Has **minimum total edge weight**. - Contains **no cycles**.

It minimizes the total cost of connecting all nodes.

------------------------------------------------------------------------

# When to Use Prim's Algorithm

Use Prim's Algorithm when: - The graph is connected. - The graph is
weighted. - You need the minimum total connection cost. - The graph is
dense. - The graph is represented using an adjacency list or matrix.

Do **not** use it for shortest path problems.

------------------------------------------------------------------------

# Where Prim's Algorithm is Used

-   Telecommunication cable layout
-   Electrical power distribution
-   Water pipeline planning
-   Road construction
-   Computer network design
-   PCB wire routing
-   Sensor network deployment

------------------------------------------------------------------------

# How Prim's Algorithm Works

1.  Start from any vertex.
2.  Mark it visited.
3.  Add all outgoing edges into a min-priority queue.
4.  Pick the minimum-weight edge leading to an unvisited vertex.
5.  Add that edge to the MST.
6.  Repeat until all vertices are included.

------------------------------------------------------------------------

# Algorithm Explanation with Visual Diagrams

## Initial Graph

``` text
        2
    A ------- B
   / \       / \
 6/   \3   8/   \5
 /     \   /     \
C-------D---------E
   1        7
```

Start at **A**

### Step 1

``` text
Visited:
[A]

Candidate edges:
A-B (2)
A-D (3)
A-C (6)

Choose:
A-B (2)
```

### Step 2

``` text
Visited:
[A,B]

Candidate:
A-D (3)
A-C (6)
B-E (5)
B-D (8)

Choose:
A-D (3)
```

### Step 3

``` text
Visited:
[A,B,D]

Candidate:
D-C (1)
B-E (5)
A-C (6)
D-E (7)

Choose:
D-C (1)
```

### Step 4

``` text
Visited:
[A,B,C,D]

Candidate:
B-E (5)
D-E (7)

Choose:
B-E (5)
```

Final MST

``` text
A---2---B
|
3
|
D---1---C

|
5
|
E

Total Cost = 11
```

------------------------------------------------------------------------

# Limitations and Edge Cases

## 1. Disconnected Graph

``` text
A---B

C---D
```

No single MST exists.

------------------------------------------------------------------------

## 2. Unweighted Graph

``` text
A---B
|   |
C---D
```

All weights equal.

Many MSTs are possible.

------------------------------------------------------------------------

## 3. Multiple MSTs

``` text
     1
A---------B
|\       /|
| \     / |
1  \   / 1
|   \ /   |
|   / \   |
|  /   \  |
C---------D
     1
```

Different edge choices can produce different MSTs with identical cost.

------------------------------------------------------------------------

## 4. Directed Graph

Prim's Algorithm does **not** work correctly.

------------------------------------------------------------------------

## 5. Very Large Sparse Graphs

Kruskal's Algorithm is often preferred.

------------------------------------------------------------------------

# Java Implementation

``` java
import java.util.*;

public class PrimsAlgorithm {

    static class Edge {
        int to;
        int weight;

        Edge(int to, int weight) {
            this.to = to;
            this.weight = weight;
        }
    }

    static class Node implements Comparable<Node> {
        int vertex;
        int weight;

        Node(int vertex, int weight) {
            this.vertex = vertex;
            this.weight = weight;
        }

        @Override
        public int compareTo(Node other) {
            return this.weight - other.weight;
        }
    }

    public static void primMST(List<List<Edge>> graph) {

        int V = graph.size();

        boolean[] visited = new boolean[V];

        PriorityQueue<Node> pq = new PriorityQueue<>();

        pq.offer(new Node(0, 0));

        int totalCost = 0;

        while (!pq.isEmpty()) {

            Node current = pq.poll();

            int u = current.vertex;

            if (visited[u])
                continue;

            visited[u] = true;

            totalCost += current.weight;

            System.out.println("Visit Vertex " + u +
                    " Cost = " + current.weight);

            for (Edge edge : graph.get(u)) {

                if (!visited[edge.to]) {
                    pq.offer(new Node(edge.to, edge.weight));
                }
            }
        }

        for (boolean v : visited) {
            if (!v) {
                System.out.println("Graph is disconnected.");
                return;
            }
        }

        System.out.println("Total MST Cost = " + totalCost);
    }

    public static void addEdge(List<List<Edge>> graph,
                               int u, int v, int w) {

        graph.get(u).add(new Edge(v, w));
        graph.get(v).add(new Edge(u, w));
    }

    public static void main(String[] args) {

        int V = 5;

        List<List<Edge>> graph = new ArrayList<>();

        for (int i = 0; i < V; i++)
            graph.add(new ArrayList<>());

        addEdge(graph,0,1,2);
        addEdge(graph,0,3,3);
        addEdge(graph,0,2,6);
        addEdge(graph,1,3,8);
        addEdge(graph,1,4,5);
        addEdge(graph,2,3,1);
        addEdge(graph,3,4,7);

        primMST(graph);
    }
}
```

------------------------------------------------------------------------

# Complexity Analysis

  Operation                  Complexity
  -------------------------- ------------
  Priority Queue insertion   O(log V)
  Edge processing            O(E log V)
  Space                      O(V + E)

Using: - Adjacency List - Priority Queue (Min Heap)

Overall:

-   **Time:** O(E log V)
-   **Space:** O(V + E)

------------------------------------------------------------------------

# Example Walkthrough

  Step   Selected Edge   Cost     Total
  ------ --------------- ------ -------
  1      A-B             2            2
  2      A-D             3            5
  3      D-C             1            6
  4      B-E             5           11

Output:

``` text
Visit Vertex 0 Cost = 0
Visit Vertex 1 Cost = 2
Visit Vertex 3 Cost = 3
Visit Vertex 2 Cost = 1
Visit Vertex 4 Cost = 5

Total MST Cost = 11
```

------------------------------------------------------------------------

# Exam Tips

-   Works only on connected, weighted, undirected graphs.
-   Produces a Minimum Spanning Tree.
-   Greedy algorithm.
-   Uses a Min Priority Queue for efficiency.
-   Time Complexity: **O(E log V)** with adjacency list + priority
    queue.
-   Difference from Kruskal:
    -   Prim grows one tree.
    -   Kruskal grows a forest and merges components.
