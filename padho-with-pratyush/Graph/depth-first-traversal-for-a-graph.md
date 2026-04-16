Prob: https://www.geeksforgeeks.org/problems/depth-first-traversal-for-a-graph/1

Sol 1: DFS (Depth First Search)

Solution Explanation:

1. Maintain a visited array to track visited vertices.
2. Maintain a result list to store DFS traversal order.
3. Start from vertex 0 (source).
4. For current vertex:
   - If not visited, add to result and mark as visited.
   - Recursively visit all unvisited neighbors.
5. Since adjacency list stores neighbors, recursion naturally explores depth-first.

Intuition: DFS explores as deep as possible before backtracking. Using recursion with adjacency list naturally achieves this - we visit a vertex, then recursively visit each of its unvisited neighbors, going deeper until no unvisited neighbors remain, then backtrack.

```java
class Solution {
    private static int size = 0;
    private static ArrayList<Integer> res = null;
    private static int[] visitedArr = null;
    
    public static void rec(ArrayList<ArrayList<Integer>> adj, int index) {
        if(visitedArr[index] == 1) return;
        res.add(index);
        visitedArr[index] = 1;
        for(int i = 0; i < adj.get(index).size(); i++) {
            int elem = adj.get(index).get(i);
            rec(adj, elem);
        }
    }
    
    public ArrayList<Integer> dfs(ArrayList<ArrayList<Integer>> adj) {
        size = adj.size();
        res = new ArrayList<>();
        visitedArr = new int[size];
        rec(adj, 0);
        return res;
    }
}
```

Time Complexity: O(V + E) where V is vertices and E is edges
Space Complexity: O(V) for visited array and recursion stack
