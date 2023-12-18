---
title: "Shortest Path Algorithms"
datePublished: Mon Dec 18 2023 10:16:42 GMT+0000 (Coordinated Universal Time)
cuid: clqarh8sk000808kz534h3jcu
slug: shortest-path-algorithms

---

# What is Shortest Path Algorithm?

The shortest path algorithms aim to find the path in a graph where the sum of edge weights is minimized.

# SSSP(Single Source Shortest Path) - Starting Vertex Exists

## 1. Dijkstra Algorithm
- **Time Complexity:** O(VElogV) - V: All nodes, E: Edges connected to each node, logV: For each edge's relaxation
    - relax ⇒ insertion and extraction-min of Min-Heap data structure
    - Considering all edges in the graph as E: E = O(V^2), so O(V^2logV)
- **Principle:** Find the shortest path by continuously following the minimum weight from the starting vertex.
- **Features:**
    - Does not allow negative weights (cannot guarantee the shortest path when negative weights exist).
    - Greedy algorithm.
    - Performs relaxation by following the minimum weight in each iteration.
- **Data Structure in Use:** Adjacency list {Vertex: [Neighbor Vertex, Neighbor Vertex…]}, Min-Heap, Fibonacci Heap  
        - Using a Fibonacci Heap achieves a time complexity of O(V^2).

## 2. Bellman-Ford Algorithm
- **Time Complexity:** O(VE) - V: All vertices
    - E = O(V^2), so O(V^3)
- **Principle:** Find the shortest path from the starting vertex to all vertices. It allows negative weights and can detect negative weight cycles.
- **Features:**
    - Allows negative weights.
    - Does not allow negative cycles but can detect them.
    - Dynamic Programming (DP) algorithm.
    - Performs relaxation by considering all edges in each iteration.
- **Data Structure in Use:** Graph edge list {Edge:(Start, End, Weight), …}

---
# All-Pairs - No Starting Vertex

## 1. Floyd-Warshall Algorithm
- **Principle:** Update the shortest paths by considering all vertices while traversing all other vertices.
- **Data Structure in Use:** Graph weight matrix
- **Time Complexity:** O(N^3) -  triple for loop
- **Features:**
    - Allows negative weights.
    - Calculates the shortest path for all pairs of vertices in one go.





## Refernces
https://www.youtube.com/watch?v=4pyBLcHXRjQ&t=72s
https://velog.io/@dlgosla/%EC%B5%9C%EB%8B%A8%EA%B2%BD%EB%A1%9C-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98
https://www.youtube.com/watch?v=YiGxC9hAg4s&t=323s
https://www.youtube.com/watch?v=JPNMSqfjjHg
