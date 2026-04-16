Prob: https://www.geeksforgeeks.org/problems/bfs-traversal-of-graph/1

Sol 1: BFS (Breadth First Search)

Solution Explanation:

1. Maintain a visited array to track visited vertices.
2. Maintain a result list to store BFS traversal order.
3. Use a queue for level-order processing.
4. Start from vertex 0, mark it visited and add to queue.
5. While queue is not empty:
   - Dequeue a vertex, add to result.
   - Enqueue all unvisited neighbors, mark them visited.
6. Process level by level until queue is empty.

Intuition: BFS explores all neighbors at current depth before moving to next level. Using a queue ensures vertices are processed in FIFO order - first visited vertices are processed first. This is ideal for finding shortest path in unweighted graphs.

```java
class Solution {
    public ArrayList<Integer> bfs(ArrayList<ArrayList<Integer>> adj) {
        Queue<Integer> queue = new LinkedList<>();
        int size = adj.size();
        boolean[] visited = new boolean[size];
        ArrayList<Integer> res = new ArrayList<>();
        
        queue.offer(0);
        visited[0] = true;
        
        while (!queue.isEmpty()) {
            int node = queue.poll();
            res.add(node);
            
            for (int neighbor : adj.get(node)) {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    queue.offer(neighbor);
                }
            }
        }
        return res;
    }
}
```

Time Complexity: O(V + E) where V is vertices and E is edges
Space Complexity: O(V) for visited array and queue
