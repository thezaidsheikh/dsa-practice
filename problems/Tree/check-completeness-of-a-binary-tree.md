Prob: https://leetcode.com/problems/check-completeness-of-a-binary-tree/submissions/1964116981/

Sol 1: Using BFS (Breadth-First Search) / Level Order Traversal

1. Start from the root node.
2. If the current node is null, return true.
3. If the current node is a leaf node (both left and right children are null), check if the current node is the last node in the traversal. If yes, return true. If not, return false.
4. If the current node is not a leaf node, recursively call checkCompleteness() on the left and right children.
5. If both the left and right children return true, return true. Otherwise, return false.

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
    public boolean isCompleteTree(TreeNode root) {
        Queue<TreeNode> q = new LinkedList<>();
        boolean isNull = false;
        q.offer(root);
        while(!q.isEmpty()) {
            TreeNode top = q.poll();
            if(top == null) isNull = true;
            else if(isNull) return false;
            else {
                q.offer(top.left);
                q.offer(top.right);
            }
        }
        return true;
    }
}
```

Time Complexity: O(n)
Space Complexity: O(w) where w is the maximum width of the tree
