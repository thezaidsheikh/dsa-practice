# Graph

## Definition

A graph is a data structure used to represent relationships between different entities.

A graph is made of:

- **Vertices / Nodes**: the entities
- **Edges**: the connections between those entities

Unlike arrays, stacks, queues, or trees, graphs are not restricted to linear or hierarchical relationships. A node can connect to many other nodes in any pattern.

## One Real-World Example To Understand Everything

We will use only one real-world example throughout this note:

**A city metro system**

Think of:

- each **metro station** as a **node**
- each **track/connection** between stations as an **edge**

Example:

```text
Central ---- Park ---- Museum
   |           |
Airport ---- CityMall
```

In this graph:

- `Central`, `Park`, `Museum`, `Airport`, `CityMall` are nodes
- each direct track between two stations is an edge

This one example is enough to explain graph types, storage, traversal, and major concepts.

## Why Graph Is Important

Graphs are important because many real-world systems are naturally networks.

Examples:

- road maps
- social networks
- computer networks
- metro systems
- web page links
- dependency graphs
- flight routes

Whenever you see:

- places connected to places
- people connected to people
- tasks depending on tasks
- devices connected to devices
- states transitioning into other states

there is a strong chance the problem can be modeled as a graph.

## Core Terminology

### Node / Vertex

Represents an entity.

In our example:

- a metro station is a node

### Edge

Represents a connection between two nodes.

In our example:

- a track between two stations is an edge

### Path

A sequence of nodes connected by edges.

Example:

```text
Airport -> Central -> Park -> Museum
```

This is a valid path from `Airport` to `Museum`.

### Degree

The number of connections a node has.

In the metro system:

- `Central` has degree `2` if connected only to `Park` and `Airport`
- if it connects to more stations, degree increases

### Cycle

A cycle exists when you can start from a node and come back to it by following edges.

Example:

```text
Central -> Park -> CityMall -> Airport -> Central
```

This forms a cycle.

### Connected Graph

A graph is connected if every node can be reached from every other node.

In the metro example, if every station is reachable from every other station somehow, the graph is connected.

## Types of Graphs

### 1. Undirected Graph

If an edge works in both directions, the graph is undirected.

Metro example:

- if a train can go from `Central` to `Park`
- and also from `Park` to `Central`

then this connection is undirected.

```text
Central ---- Park
```

Most normal road or track connectivity examples are modeled this way unless direction matters.

### 2. Directed Graph

If edges have direction, the graph is directed.

Metro example:

- some special shuttle may go from `Airport` to `Central`
- but not in reverse

```text
Airport -> Central
```

This is useful when movement is one-way.

### 3. Weighted Graph

If each edge stores a cost, distance, time, or price, the graph is weighted.

Metro example:

- `Central -> Park` may take `5` minutes
- `Park -> Museum` may take `3` minutes

```text
Central --5-- Park --3-- Museum
```

This is used when the cost of traveling matters.

### 4. Unweighted Graph

If all edges are treated equally, the graph is unweighted.

Metro example:

- we only care whether a station is connected
- we do not care about travel time

This is common in BFS-based shortest path for minimum number of stops.

### 5. Cyclic Graph

A graph containing at least one cycle.

Metro example:

```text
Central -> Park -> CityMall -> Airport -> Central
```

This helps provide alternative routes.

### 6. Acyclic Graph

A graph with no cycles.

In a metro setting, this would be unusual, but if every expansion only branches outward with no route loops, it could become acyclic.

Directed acyclic graphs are called **DAGs** and are very important in dependency problems.

### 7. Connected and Disconnected Graph

### Connected

Every station is reachable.

### Disconnected

Some stations belong to isolated groups.

Example:

- one metro line is not connected to the rest of the city network

## Internal Working of a Graph

A graph is usually stored in one of two ways:

- adjacency list
- adjacency matrix

### 1. Adjacency List

This is the most common representation in DSA problems.

Each node stores a list of its neighbors.

Metro example:

```text
Central  -> [Park, Airport]
Park     -> [Central, Museum, CityMall]
Museum   -> [Park]
Airport  -> [Central, CityMall]
CityMall -> [Park, Airport]
```

### Why It Is Good

- space-efficient for sparse graphs
- easy to iterate over neighbors
- best choice for BFS and DFS in most coding problems

### Complexity

- Space: `O(V + E)`

where:

- `V` = number of vertices
- `E` = number of edges

### 2. Adjacency Matrix

Use a `V x V` matrix where:

- `matrix[i][j] = 1` or weight if edge exists
- `matrix[i][j] = 0` or infinity if edge does not exist

Metro example:

```text
          C   P   M   A   CM
Central   0   1   0   1   0
Park      1   0   1   0   1
Museum    0   1   0   0   0
Airport   1   0   0   0   1
CityMall  0   1   0   1   0
```

### Why It Is Good

- very easy to check whether a direct edge exists
- useful when graph is dense

### Complexity

- Space: `O(V^2)`

## Adjacency List vs Adjacency Matrix

| Representation | Space | Edge Check | Neighbor Traversal | Best Use |
| --- | --- | --- | --- | --- |
| Adjacency List | `O(V + E)` | `O(degree)` or more | efficient | sparse graphs |
| Adjacency Matrix | `O(V^2)` | `O(1)` | `O(V)` | dense graphs |

In interview problems, adjacency list is usually preferred.

## Important Graph Methods / Operations

These are the common operations performed on graphs.

### 1. Add Vertex

Add a new node to the graph.

Metro example:

- a new station `Harbor` opens

Time Complexity:

- adjacency list: `O(1)` average
- adjacency matrix: expensive if matrix must be resized

### 2. Add Edge

Connect two nodes.

Metro example:

- add a new track between `Museum` and `CityMall`

Time Complexity:

- adjacency list: `O(1)` average
- adjacency matrix: `O(1)`

### 3. Remove Edge

Remove a connection.

Metro example:

- track under maintenance between `Central` and `Airport`

Time Complexity:

- adjacency list: often `O(degree)`
- adjacency matrix: `O(1)`

### 4. Check If Two Nodes Are Directly Connected

Example:

- is there a direct track from `Central` to `Park`?

Time Complexity:

- adjacency list: `O(degree of node)`
- adjacency matrix: `O(1)`

### 5. Traverse the Graph

Used to visit all reachable nodes.

The two most important traversal methods are:

- BFS
- DFS

### Breadth-First Search (BFS)

BFS explores level by level.

In the metro system:

- if you start at `Central`
- BFS first visits all stations exactly one stop away
- then all stations two stops away
- then three stops away, and so on

### Why BFS Is Useful

- shortest path in an unweighted graph
- minimum number of stops
- level-wise traversal
- connectivity checking

### Example

Start from `Central`:

```text
Level 0: Central
Level 1: Park, Airport
Level 2: Museum, CityMall
```

If the question is:

**What is the minimum number of stops from `Central` to `Museum`?**

BFS gives the answer naturally.

### BFS Working

1. Put the starting node into a queue.
2. Mark it visited.
3. Remove one node from the queue.
4. Visit all unvisited neighbors.
5. Push them into the queue.
6. Repeat until queue becomes empty.

### BFS Complexity

- Time: `O(V + E)`
- Space: `O(V)`

### Java Outline

```java
Queue<Integer> q = new LinkedList<>();
boolean[] visited = new boolean[n];

q.offer(start);
visited[start] = true;

while (!q.isEmpty()) {
    int node = q.poll();

    for (int neighbor : graph.get(node)) {
        if (!visited[neighbor]) {
            visited[neighbor] = true;
            q.offer(neighbor);
        }
    }
}
```

### Depth-First Search (DFS)

DFS goes as deep as possible before backtracking.

In the metro system:

- from `Central`, it may go to `Park`
- then `Museum`
- then backtrack
- then explore another route

### Why DFS Is Useful

- path exploration
- cycle detection
- connected components
- topological sort
- backtracking-style graph problems

### DFS Working

1. Start from a node.
2. Visit one unvisited neighbor.
3. Continue deeper until no more unvisited neighbors remain.
4. Backtrack and continue.

### DFS Complexity

- Time: `O(V + E)`
- Space: `O(V)` due to recursion stack or explicit stack

### Java Outline

```java
void dfs(int node, List<List<Integer>> graph, boolean[] visited) {
    visited[node] = true;

    for (int neighbor : graph.get(node)) {
        if (!visited[neighbor]) {
            dfs(neighbor, graph, visited);
        }
    }
}
```

## Other Important Graph Concepts

### 1. Visited Array / Set

This is extremely important in graph traversal.

Why:

- prevents infinite loops in cyclic graphs
- avoids re-processing nodes
- helps maintain `O(V + E)` complexity

In the metro graph, without visited tracking, the traversal may keep moving in circles forever.

### 2. Connected Components

A connected component is a group of nodes where every node is reachable from every other node in that group.

Metro example:

- if one suburban line is totally disconnected from the main city network, it forms another component

To count connected components:

- run BFS or DFS from every unvisited node

### 3. Shortest Path

In graphs, shortest path depends on the graph type.

### Unweighted Graph

Use BFS.

Metro example:

- minimum number of stops between stations

### Weighted Graph

Use algorithms like:

- Dijkstra
- Bellman-Ford

Metro example:

- minimum travel time instead of minimum stops

### 4. Cycle Detection

Important when checking whether a graph contains loops.

Use cases:

- route loops
- dependency validation
- checking whether a graph is a tree

Cycle detection methods:

- undirected graph: DFS/BFS with parent tracking
- directed graph: DFS with path tracking or Kahn's algorithm

### 5. Topological Sort

Topological sort applies only to a **Directed Acyclic Graph (DAG)**.

It gives an order such that every dependency comes before the dependent task.

Metro systems are not the perfect real-world fit for topological sort, but the graph concept still matters in other systems like course prerequisites or build dependencies.

Important methods:

- DFS-based topological sort
- Kahn's algorithm using indegree

### 6. Bipartite Graph

A graph is bipartite if nodes can be split into two groups such that no edge connects two nodes in the same group.

This is commonly checked using BFS or DFS coloring.

In a transport-style analogy, imagine alternate types of stops or partitions, though this concept is more common in matching problems than metro routing.

## Common Graph Algorithms

| Algorithm | Main Use | Time Complexity |
| --- | --- | --- |
| BFS | shortest path in unweighted graph, connectivity | `O(V + E)` |
| DFS | traversal, cycle detection, components | `O(V + E)` |
| Dijkstra | shortest path in weighted graph with non-negative weights | `O((V + E) log V)` with heap |
| Bellman-Ford | shortest path with negative edges | `O(V * E)` |
| Floyd-Warshall | all-pairs shortest path | `O(V^3)` |
| Topological Sort | ordering in DAG | `O(V + E)` |
| Union-Find | dynamic connectivity / cycle checks | near `O(1)` amortized per operation |

## Real-World Example End-to-End

Suppose the city wants to answer these questions:

1. Can I travel from `Airport` to `Museum`?
2. What is the minimum number of stops?
3. What is the fastest route if each edge has travel time?
4. Is the whole metro network connected?
5. If one line is closed, which stations become isolated?

Graph modeling solves all of these:

- stations become nodes
- tracks become edges
- BFS finds minimum stops
- Dijkstra finds minimum travel time
- DFS/BFS checks connectivity and components
- weighted edges model actual travel times

This is why graph is one of the most practical and important DSA topics.

## Important Takeaways / Key Points

- A graph models relationships, networks, and connectivity.
- Nodes represent entities and edges represent connections.
- Graphs can be directed, undirected, weighted, unweighted, cyclic, or acyclic.
- Adjacency list is the most common representation in coding interviews.
- BFS is best for shortest path in unweighted graphs.
- DFS is best for deep traversal, cycle detection, and component exploration.
- Always use a visited array or set unless the problem specifically guarantees otherwise.
- Graph problems often hide behind words like network, route, dependency, relation, or connection.
- If the problem asks about reachability, path, connectivity, shortest route, or dependency order, graph should come to mind immediately.

## When To Think About Graph

Ask these questions:

- Are objects connected to other objects?
- Can I move from one state/place/node to another?
- Do I need to know whether something is reachable?
- Do I need the shortest route or least cost path?
- Are dependencies involved?
- Is the input naturally a network?

If yes, graph is a strong candidate.

## Complexity Summary

| Concept / Operation | Time | Space |
| --- | --- | --- |
| Store graph as adjacency list | build usually `O(V + E)` | `O(V + E)` |
| Store graph as adjacency matrix | build `O(V^2)` typically | `O(V^2)` |
| BFS | `O(V + E)` | `O(V)` |
| DFS | `O(V + E)` | `O(V)` |
| Add edge in adjacency list | `O(1)` average | `O(1)` extra |
| Add edge in adjacency matrix | `O(1)` | `O(1)` extra |
| Check edge in adjacency matrix | `O(1)` | `O(1)` |
| Check edge in adjacency list | `O(degree)` | `O(1)` |

## Key Insight

A graph is the right mental model whenever the problem is really about connections, not just values.
