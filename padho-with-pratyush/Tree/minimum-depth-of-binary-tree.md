Prob: https://leetcode.com/problems/minimum-depth-of-binary-tree/

Sol 1: Optimal approach - Using BFS (Level Order Traversal)

1. Start from the root node.
2. If the current node is null, return 0.
3. If the current node is a leaf node (both left and right children are null), return 1.
4. If the current node has only one child, return the minimum depth of that child.
5. If the current node has two children, return the minimum depth of both children.
6. Repeat steps 2-5 until the minimum depth is found.

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
    public static int rec(TreeNode root) {
        if(root == null) return 0;

        // If it's a leaf node, min depth is 1 (just this node)
        if (root.left == null && root.right == null) {
            return 1;
        }

        // If one child is null, you MUST go through the other child
        if (root.left == null) {
            return 1 + rec(root.right);
        }
        if (root.right == null) {
            return 1 + rec(root.left);
        }

        int lh = rec(root.left);
        int rh = rec(root.right);
        return 1 + Math.min(lh,rh);
    }
    public int minDepth(TreeNode root) {
        return rec(root);
    }
}
```

Time Complexity: O(n)
Space Complexity: O(n)
