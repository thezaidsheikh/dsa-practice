Prob: https://leetcode.com/problems/binary-tree-level-order-traversal-ii/description/

Sol 1: Optimal approach - Using BFS
1. Create a queue to store nodes
2. Initialize an empty list to store the result
3. If root is null, return the result
4. Add root to the queue
5. While queue is not empty
   - Get the size of the queue
   - Initialize a list to store the elements of the current level
   - While size is greater than 0
     - Get the front node from the queue
     - Add the node's value to the current level list
     - If the node's left child is not null, add it to the queue
     - If the node's right child is not null, add it to the queue
   - Add the current level list to the result
6. Return the result
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        Queue<TreeNode> pq = new LinkedList<>();
        List<List<Integer>> res = new ArrayList<>();
        if (root == null)
            return res;
        pq.offer(root);
        while (!pq.isEmpty()) {
            int size = pq.size();
            List<Integer> level = new ArrayList<>();
            while (size-- > 0) {
                TreeNode node = pq.poll();
                level.add(node.val);

                if (node.left != null)
                    pq.offer(node.left);
                if (node.right != null)
                    pq.offer(node.right);
            }

            res.add(level);
        }
        Collections.reverse(res);
        return res;
    }
}
```
Time complexity - O(N),
Space complexity - O(N)