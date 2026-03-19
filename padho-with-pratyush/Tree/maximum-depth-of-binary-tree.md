Prob: https://leetcode.com/problems/maximum-depth-of-binary-tree/

Sol 1: Optimal approach - Using DFS (Depth First Search)

1. Start from the root node.
2. If the current node is null, return 0.
3. If the current node is a leaf node (both left and right children are null), return 1.
4. If the current node has only one child, return the maximum depth of that child.
5. If the current node has two children, return the maximum depth of both children.
6. Repeat steps 2-5 until the maximum depth is found.

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
    public int maxDepth(TreeNode root) {
        if(root == null) return 0;
        int lh = maxDepth(root.left);
        int rh = maxDepth(root.right);
        return 1 + Math.max(lh,rh);
    }
}
```
