# Kahn's Algorithm

## Definition

Kahn's Algorithm is a BFS-based method for computing **topological order** of a Directed Acyclic Graph (DAG). It uses the concept of **in-degree** to determine the order of nodes.

## When To Use Kahn's Algorithm

Use Kahn's Algorithm when:
- You need topological sort (dependency ordering)
- You prefer iterative over recursive solutions
- You need to detect cycles in a directed graph
- The problem involves "peeling off" elements with no dependencies
- Common scenarios: course scheduling, build systems, task dependencies

## Why It Works

1. **In-degree 0 nodes** have no dependencies - they must come first
2. Once we process (remove) these nodes, we reduce the in-degree of their neighbors
3. New nodes may become in-degree 0 after their dependencies are removed
4. If we can process all nodes → valid topological order exists
5. If nodes remain with in-degree > 0 → cycle exists (impossible to order)

## Algorithm Steps

1. **Calculate in-degree** for all nodes
2. **Initialize queue** with all nodes having in-degree 0
3. **Process queue**:
   - Dequeue node, add to result
   - For each neighbor, decrement its in-degree
   - If neighbor's in-degree becomes 0, enqueue it
4. **Check result**: if result size < V, cycle exists

## Visual Example

```text
Graph:
    0 → 1 → 2
    ↓   ↓
    3 → 4

Step 1: Calculate in-degrees
    0: 0, 1: 1, 2: 1, 3: 1, 4: 2

Step 2: Queue starts with [0] (in-degree 0)

Step 3: Process
    - Dequeue 0 → result: [0]
      Reduce in-degree(1): 1→0, reduce in-degree(3): 1→0
      Queue: [1, 3]
    
    - Dequeue 1 → result: [0, 1]
      Reduce in-degree(2): 1→0
      Queue: [3, 2]
    
    - Dequeue 3 → result: [0, 1, 3]
      Reduce in-degree(4): 2→1
      Queue: [2]
    
    - Dequeue 2 → result: [0, 1, 3, 2]
      in-degree(4): 1→0
      Queue: [4]
    
    - Dequeue 4 → result: [0, 1, 3, 2, 4]

Step 4: All 5 nodes processed → No cycle!
```

## Java Implementation

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
            return new int[0];
        }
        
        return order;
    }
}
```

## Key Points

- **In-degree**: number of incoming edges to a node
- **Queue**: holds nodes with no pending dependencies
- **Zero in-degree = no prerequisites = comes first**
- Cycle detection: if result size < V, cycle exists

## Complexity

| Aspect | Complexity |
| --- | --- |
| Time | `O(V + E)` |
| Space | `O(V + E)` for graph + `O(V)` for queue |

## Kahn's vs DFS Topological Sort

| Kahn's | DFS-based |
| --- | --- |
| Iterative (queue) | Recursive |
| In-degree based | Post-order reverse |
| Cycle: remaining nodes | Cycle: visited state |
| Natural BFS order | Natural DFS order |
| Easy to understand | Slightly more complex |

## Common Problems Using Kahn's

1. Course Schedule (LeetCode 207)
2. Course Schedule II (LeetCode 210)
3. Alien Dictionary (LeetCode 269)
4. Minimum Height Trees (LeetCode 310)
5. Find Safe States in Directed Graph (LeetCode 802)
