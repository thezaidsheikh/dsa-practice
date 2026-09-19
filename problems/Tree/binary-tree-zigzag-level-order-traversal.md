Prob: https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/description/

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
   - If the level number is odd, reverse the current level list
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
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        List<List<Integer>> ls = new ArrayList<>();
        boolean isRight = true;
        if (root != null) {
            queue.offer(root); // add root node into queue
        }

        while (!queue.isEmpty()) {
            List<Integer> list = new ArrayList<>();
            int size = queue.size();
            while (size-- > 0) {
                TreeNode t = queue.peek();
                list.add(queue.poll().val);
                if (t.left != null) queue.offer(t.left);
                if (t.right != null) queue.offer(t.right);
            }
            if (!isRight) {
                Collections.reverse(list);
            }
            ls.add(list);
            isRight = !isRight;
        }
        return ls;
    }
}
```
Time complexity - O(N)
Space complexity - O(N)