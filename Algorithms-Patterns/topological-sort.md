# Topological Sort

## Definition

Topological sort is a linear ordering of nodes in a **Directed Acyclic Graph (DAG)** such that for every directed edge `u -> v`, node `u` comes before node `v` in the ordering.

In simpler terms: **it orders tasks so that dependencies come before the dependent tasks.**

## When To Use Topological Sort

Think of topological sort when you see these problem patterns:

1. **Course Prerequisites**
   - "Complete all courses in correct order given prerequisites"

2. **Build Systems / Dependency Resolution**
   - "In what order should packages be built/installed?"
   - "Resolve dependencies in correct order"

3. **Task Scheduling**
   - "Complete tasks respecting dependencies between them"

4. **Instructions / Sequence Problems**
   - "Find the correct sequence of events/instructions"

5. **Package Management**
   - "When installing packages, what order ensures all dependencies are met?"

### Interview Recognition Cue

If the problem mentions:
- "must complete X before Y"
- "dependency on"
- "prerequisite"
- "order of installation"
- "build sequence"
- Any scenario with "comes before" relationships

Think: **DAG + Topological Sort**

## DAG (Directed Acyclic Graph)

### What Makes A Graph A DAG

- **Directed**: edges have a specific direction (u -> v, not necessarily v -> u)
- **Acyclic**: no cycles exist (cannot start at a node and return to it by following directed edges)

### Real-World Example

Course prerequisites:

```text
Intro to Programming
    ↓
Data Structures
    ↓
Algorithms
    ↓
System Design
```

Each course depends on the previous one. You cannot complete System Design before Data Structures.

## Two Methods For Topological Sort

### Method 1: DFS-Based Topological Sort

**Intuition**: In a DAG, if we do DFS and record nodes in reverse order of their completion (when backtracking), we get a valid topological order.

**Why It Works**:
- In DFS, we visit a node and all its descendants
- When we finish processing a node (after all neighbors are processed), we add it to the result
- Recording nodes **after** processing all neighbors gives reverse topological order
- Reversing this gives topological order

**Steps**:
1. Build adjacency list from graph
2. Use DFS to traverse
3. Track visited nodes to avoid cycles
4. Add node to result when DFS completes (post-order)
5. Reverse the result at the end

**Java Implementation**:

```java
class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        List<List<Integer>> graph = new ArrayList<>();
        for (int i = 0; i < numCourses; i++) {
            graph.add(new ArrayList<>());
        }
        
        for (int[] prereq : prerequisites) {
            graph.get(prereq[1]).add(prereq[0]);
        }
        
        int[] visited = new int[numCourses]; // 0=unvisited, 1=visiting, 2=visited
        List<Integer> result = new ArrayList<>();
        
        for (int i = 0; i < numCourses; i++) {
            if (dfs(i, graph, visited, result)) {
                return new int[0]; // cycle detected
            }
        }
        
        int[] order = new int[numCourses];
        for (int i = 0; i < numCourses; i++) {
            order[i] = result.get(i);
        }
        return order;
    }
    
    private boolean dfs(int node, List<List<Integer>> graph, 
                    int[] visited, List<Integer> result) {
        if (visited[node] == 1) return true; // cycle
        if (visited[node] == 2) return false; // already processed
        
        visited[node] = 1; // visiting
        
        for (int neighbor : graph.get(node)) {
            if (dfs(neighbor, graph, visited, result)) {
                return true; // cycle detected
            }
        }
        
        visited[node] = 2; // visited done
        result.add(node); // add to result after all neighbors processed
        
        return false;
    }
}
```

**Time Complexity**: `O(V + E)`
**Space Complexity**: `O(V + E)` for graph + `O(V)` for recursion stack

### Method 2: Kahn's Algorithm (BFS-Based)

**Intuition**: Start with nodes having no incoming edges (in-degree = 0). Remove them, add to result, and reduce in-degree of neighbors. Repeat.

**Why It Works**:
- Nodes with in-degree 0 have no dependencies
- These must come first in topological order
- After removing them, new nodes may become in-degree 0
- Continue until all nodes are processed

**Steps**:
1. Build adjacency list and calculate in-degree for each node
2. Initialize queue with all nodes having in-degree 0
3. While queue is not empty:
   - Remove node from queue, add to result
   - For each neighbor, reduce its in-degree by 1
   - If neighbor's in-degree becomes 0, add to queue
4. If result contains all nodes, no cycle exists
5. Otherwise, cycle exists

**Java Implementation**:

```java
class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        List<List<Integer>> graph = new ArrayList<>();
        int[] inDegree = new int[numCourses];
        
        for (int i = 0; i < numCourses; i++) {
            graph.add(new ArrayList<>());
        }
        
        for (int[] prereq : prerequisites) {
            graph.get(prereq[1]).add(prereq[0]);
            inDegree[prereq[0]]++;
        }
        
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) {
            if (inDegree[i] == 0) {
                queue.offer(i);
            }
        }
        
        int[] order = new int[numCourses];
        int count = 0;
        
        while (!queue.isEmpty()) {
            int node = queue.poll();
            order[count++] = node;
            
            for (int neighbor : graph.get(node)) {
                inDegree[neighbor]--;
                if (inDegree[neighbor] == 0) {
                    queue.offer(neighbor);
                }
            }
        }
        
        if (count != numCourses) {
            return new int[0]; // cycle exists
        }
        
        return order;
    }
}
```

**Time Complexity**: `O(V + E)`
**Space Complexity**: `O(V + E)`

## Comparison: DFS vs Kahn's

| Aspect | DFS | Kahn's Algorithm |
| --- | --- | --- |
| Approach | Recursive | Iterative with queue |
| Cycle Detection | Easy (back-edge) | Easy (unprocessed nodes remain) |
| Order | Reverse post-order | In-degree 0 first |
| Space | recursion stack | queue |
| Intuition | "finish before dependent" | "start with no dependencies" |
| Preferred When | Need post-order anyway | Streaming/fIFO preferred |

## Important Concepts

### 1. Cycle Detection

Topological sort **only works on DAGs**.

If a cycle exists:
- No valid topological order exists
- DFS-based: detect via visited state (visiting node again = cycle)
- Kahn's: if result size < V, cycle exists

**Example of Cycle**:

```text
A -> B -> C -> A
```

This is impossible to order topologically.

### 2. In-Degree

In-degree of a node = number of edges pointing TO the node.

In topological sort:
- Nodes with in-degree 0 come first
- After processing, reduce neighbors' in-degrees

### 3. Multiple Valid Orders

A graph may have **multiple valid topological orders**.

Example:
- A and B both have in-degree 0
- Either A before B or B before A is valid

Both are correct.

## Common LeetCode Problems

1. **Course Schedule** (LeetCode 207)
   - Determine if all courses can be completed
   - Find valid order if possible

2. **Course Schedule II** (LeetCode 210)
   - Return one valid ordering

3. **Alien Dictionary** (LeetCode 269)
   - Find character order from given words

4. **Sequence Reconstruction** (LeetCode 444)
   - Check if sequence can be reconstructed

5. **Minimum Height Trees** (LeetCode 310)
   - Use topological sort approach

## Key Takeaways

- Topological sort only works on DAGs
- It orders nodes so dependencies come first
- Use DFS (reverse post-order) or Kahn's (in-degree 0 nodes)
- Always check for cycles first
- Multiple valid orders often exist
- Kahn's is essentially "peeling onion" - remove nodes with no dependencies

## Interview Checklist

- [ ] Recognize problem as topological sort from wording
- [ ] Verify graph is a DAG (or check for cycles first)
- [ ] Choose method: DFS or Kahn's
- [ ] Handle cycle detection case
- [ ] Handle multiple valid orderings if needed
- [ ] Consider edge cases: empty graph, single node, disconnected components

## Complexity Summary

| Operation | Time | Space |
| --- | --- | --- |
| Build graph | `O(V + E)` | `O(V + E)` |
| DFS topological sort | `O(V + E)` | `O(V)` recursion |
| Kahn's algorithm | `O(V + E)` | `O(V)` queue |
| Cycle detection | included | same |