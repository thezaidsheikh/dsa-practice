Prob: https://www.geeksforgeeks.org/problems/print-adjacency-list-1587115620/1

Sol 1: Building Adjacency List from Edges

Solution Explanation:

1. Create a list of empty lists, one for each vertex (0 to V-1).
2. For each edge [src, dest] in the edges array:
   - Add `dest` to `src`'s adjacency list
   - Add `src` to `dest`'s adjacency list
   - This makes the graph undirected
3. Return the final adjacency list.

Intuition: Given a list of edges, we need to convert them into adjacency list format. Each vertex maintains a list of its neighbors. Since the edges are bidirectional (undirected graph), we add both directions.

```java
class Solution {
    public List<List<Integer>> printGraph(int V, int edges[][]) {
        List<List<Integer>> adjList = new ArrayList<>();
        
        for(int i = 0; i < V; i++) {
            adjList.add(new ArrayList<>());
        }
        
        for(int i = 0; i < edges.length; i++) {
            int src = edges[i][0];
            int dest = edges[i][1];
            
            adjList.get(src).add(dest);
            adjList.get(dest).add(src);
        }
        return adjList;
    }
}
```

Time Complexity: O(V + E) where V is vertices and E is edges
Space Complexity: O(V + E) for storing the adjacency list
