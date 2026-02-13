Prob: https://www.geeksforgeeks.org/problems/merge-k-sorted-arrays/1

Sol 1: Optimal - Using Min heap
1. Create a min heap of size k.
2. Insert the first element of each array into the min heap.
3. Fetch the minimum element from the min heap and insert the next element of the same array into the min heap.
4. Repeat steps 2 and 3 until the min heap is empty.
5. Return the merged array.
```java
class Solution {
    static class Node {
        int value;
        int row;
        int col;

        Node(int v, int r, int c) {
            value = v;
            row = r;
            col = c;
        }
    }

    public ArrayList<Integer> mergeArrays(int[][] mat) {
        ArrayList<Integer> res = new ArrayList<>();
        int n = mat.length;
        int m = mat[0].length;
        PriorityQueue<Node> pq = new PriorityQueue<>((node1,node2)->{
            return node1.value - node2.value;
        });
        
        // Insert first element in priorityQueue
        for(int i = 0; i < n; i++) {
            pq.offer(new Node(mat[i][0],i,0));
        }
        
        while(!pq.isEmpty()) {
            Node top = pq.poll();
            res.add(top.value);
            int row = top.row;
            int col = top.col;
            if(col >= m - 1) continue;
            pq.offer(new Node(mat[row][col+1],row,col+1));
        }
        return res;
    }
}
```
Time complexity = O(NlogK)
Space complexity = O(K)