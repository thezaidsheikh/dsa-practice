Prob: https://www.geeksforgeeks.org/problems/shortest-path-in-undirected-graph-having-unit-distance/1

## Solution: BFS

### Solution Explanation

1. Build adjacency list from edges
2. Initialize distance array with -1 (unvisited)
3. Set source distance to 0, add to queue
4. BFS: for each node, update unvisited neighbors with dist[current] + 1
5. Return distance array

### Intuition

In an unweighted graph with unit distances, BFS naturally finds the shortest path. BFS explores level by level:
- Level 0: source node (distance 0)
- Level 1: neighbors (distance 1)
- Level 2: neighbors of neighbors (distance 2)

The first time we reach a node is via the shortest path.

### Key Points

- Time: O(V + E)
- Space: O(V)
- Use BFS for unit weight graphs
- Initialize distances as -1 to track unvisited nodes

```java
class Solution {
    public int[] shortestPath(int V, int[][] edges, int src) {
        // 1. Build Adjacency List (Use ArrayList for dynamic neighbors)
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < V; i++) adj.add(new ArrayList<>());
        
        for (int[] edge : edges) {
            adj.get(edge[0]).add(edge[1]);
            adj.get(edge[1]).add(edge[0]);
        }

        // 2. Initialize Distances (Use -1 for unreachable nodes)
        int[] dist = new int[V];
        Arrays.fill(dist, -1);
        
        // 3. BFS Setup
        Queue<Integer> queue = new LinkedList<>();
        dist[src] = 0; // Source distance is 0
        queue.offer(src);

        while (!queue.isEmpty()) {
            int curr = queue.poll();

            for (int neighbor : adj.get(curr)) {
                // If not visited (distance is still -1)
                if (dist[neighbor] == -1) {
                    dist[neighbor] = dist[curr] + 1;
                    queue.offer(neighbor);
                }
            }
        }
        
        return dist;
    }
}
```

Time Complexity: O(V + E)
Space Complexity: O(V)