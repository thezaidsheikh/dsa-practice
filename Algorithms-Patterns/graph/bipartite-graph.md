# Bipartite Graph

## Definition

A bipartite graph is a graph whose vertices can be split into two disjoint sets such that every edge connects a vertex in one set to a vertex in the other set. No edge exists between two vertices in the same set.

In simpler terms: **you can color the graph using two colors where adjacent nodes always have different colors.**

## When To Use

- Keywords: "two groups", "partition", "can be split into two sets", "no two in same group"
- Problem types: Graph validation, matching problems, relationship problems
- Recognition: Problem asks whether something can be divided into two groups with specific constraints

### Common Problem Scenarios

1. **Relationship Problems**
   - "Can we split people into two groups where enemies are never in the same group?"
   - "Do bipartite matching exist?"

2. **Graph Partitioning**
   - "Is the graph bipartite?"
   - "Can we color the graph with 2 colors?"

3. **Matching Problems**
   - Job assignments (worker to job)
   - Marriage problems (men to women)
   - Assignment problems

## Why It Works

The fundamental insight is: **if you can 2-color a graph, it's bipartite**.

How to check:
- Start with any node, color it color A
- All neighbors must be color B
- Their neighbors must be color A
- Continue... if you ever need to color a node both colors, it's not bipartite

This works because:
- Bipartite = no odd-length cycles
- 2-coloring succeeds iff no odd cycles exist
- DFS/BFS coloring naturally detects conflicts

## Real-World Example

**Dating App Compatibility**

Imagine a dating app where:
- Some users are enemies (should never match)
- Others are compatible

```
Users: Alice, Bob, Charlie, Dave, Eve, Frank
Enemies (edges):
    Alice - Bob
    Bob - Charlie
    Charlie - Dave
    Dave - Eve
    Eve - Frank
```

Can we split users into two groups (Group A: "even positions", Group B: "odd positions") so enemies are always in different groups?

```
Group A: Alice, Charlie, Frank
Group B: Bob, Dave, Eve
```

Check: Every enemy pair crosses groups? Yes! This is bipartite.

## Algorithm Steps (BFS Coloring)

1. Create color array: -1 (uncolored) for all nodes
2. For each uncolored node:
   - Set color to 0 (say, red)
   - Add to queue
3. While queue not empty:
   - Dequeue node
   - For each neighbor:
     - If uncolored: set to opposite color, enqueue
     - If same color: NOT bipartite - return false
4. Return true (all nodes colored successfully)

## Algorithm Steps (DFS Coloring)

1. Create color array: -1 (uncolored) for all nodes
2. For each uncolored node:
   - Call dfs(node, color 0)
3. In dfs(node, color):
   - Set node color
   - For each neighbor:
     - If uncolored: dfs(neighbor, opposite color)
     - If same color: return false (not bipartite)
4. Return true

## Visual Example

```
Graph with cycle (even):
    
    A --- B
    |     |
    D --- C

Coloring:
    A: Red
    B: Blue
    C: Red (neighbors: B, D → both Blue)
    D: Blue (neighbors: A, C → both Red)

Result: BIPARTITE ✓


Graph with cycle (odd):
    
    A --- B
    |     |
    D --- C

Oops, make it triangle:
    A --- B
    |  \  |
    C --- (missing edge, let's fix)

Actually, triangle A-B-C-A:
    A: Red
    B: Blue (neighbor of A)
    C: Red (neighbor of B, but also neighbor of A!)
    
Conflict: C needs both Red and Blue → NOT BIPARTITE ✗
```

## Variations

### 1. Disconnected Graph
- Must check each connected component
- Some components may be bipartite, others not

### 2. Bipartite Matching
- After confirming bipartite, find maximum matching
- Use Hopcroft-Karp or Hungarian algorithm

### 3. Complete Bipartite Graph (Km,n)
- Every node in set A connects to every node in set B
- Common in assignment problems

## Code Implementation (BFS)

```java
class Solution {
    public boolean isBipartite(int[][] graph) {
        int n = graph.length;
        int[] color = new int[n];
        Arrays.fill(color, -1);
        
        for (int i = 0; i < n; i++) {
            if (color[i] == -1) {
                Queue<Integer> queue = new LinkedList<>();
                queue.offer(i);
                color[i] = 0;
                
                while (!queue.isEmpty()) {
                    int node = queue.poll();
                    
                    for (int neighbor : graph[node]) {
                        if (color[neighbor] == -1) {
                            color[neighbor] = color[node] ^ 1;
                            queue.offer(neighbor);
                        } else if (color[neighbor] == color[node]) {
                            return false;
                        }
                    }
                }
            }
        }
        return true;
    }
}
```

## Code Implementation (DFS)

```java
class Solution {
    public boolean isBipartite(int[][] graph) {
        int n = graph.length;
        int[] color = new int[n];
        Arrays.fill(color, -1);
        
        for (int i = 0; i < n; i++) {
            if (color[i] == -1 && !dfs(i, 0, graph, color)) {
                return false;
            }
        }
        return true;
    }
    
    private boolean dfs(int node, int c, int[][] graph, int[] color) {
        color[node] = c;
        
        for (int neighbor : graph[node]) {
            if (color[neighbor] == -1) {
                if (!dfs(neighbor, c ^ 1, graph, color)) {
                    return false;
                }
            } else if (color[neighbor] == c) {
                return false;
            }
        }
        return true;
    }
}
```

## Complexity Analysis

- Time: O(V + E) - each node and edge visited once
- Space: O(V) - color array + queue/stack

## Comparison

| Method | Pros | Cons |
| --- | --- | --- |
| BFS | Level by level, easy to visualize | Queue overhead |
| DFS | Recursive, less code | Stack overflow risk for large graphs |
| Union-Find | Can work incrementally | More complex |

## Common Problems

1. **Is Graph Bipartite?** (LeetCode 785)
   - Check if graph can be 2-colored

2. **Possible Bipartition** (LeetCode 886)
   - Given "dislike" pairs, can we split into two groups?

3. **Exam Room** (LeetCode)
   - Partition students for exams

## Interview Key Points

- Always check for odd cycles - that's what makes a graph non-bipartite
- Handle disconnected components - check each component
- Mention time/space complexity first
- Consider edge case: empty graph (trivially bipartite)
- Consider edge case: single node (trivially bipartite)
- Follow-up: What if you need to return the actual partition?

## Way To Remember

**"Two colors, no same neighbors"**

Visual: Think of a checkerboard - adjacent squares are always different colors.

**Rule**: If you can paint it like a checkerboard, it's bipartite. If you ever need to paint two adjacent squares the same color, it's not.

## Common Pitfalls

1. **Forgetting disconnected components**
   - Always loop through all nodes, not just node 0

2. **Wrong initial color**
   - Either 0/1 or Red/Blue works, just be consistent

3. **Not handling empty graph**
   - Empty graph is technically bipartite

4. **Confusing bipartite with matching**
   - Bipartite = property of graph
   - Matching = finding maximum pairs after confirming bipartite