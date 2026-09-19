Prob: https://leetcode.com/problems/diameter-of-binary-tree/description/

Sol 1: Using DFS (Depth-First Search)

1. Start from the root node.
2. If the current node is null, return 0.
3. Recursively call findDiameter() on the left and right children.
4. Calculate the diameter of the left and right subtrees.
5. Return the maximum of the diameter of the left and right subtrees, and the sum of the heights of the left and right subtrees.

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
    private static int res = 0;
    public static int rec(TreeNode root) {
        if(root == null) return 0;

        int leftHeight = rec(root.left);
        int rightHeight = rec(root.right);
        int sum = leftHeight + rightHeight;
        res = Math.max(res, sum);
        return 1 + Math.max(leftHeight, rightHeight);
    }
    public int diameterOfBinaryTree(TreeNode root) {
        res = 0;
        rec(root);
        return res;
    }
}
```

Time Complexity: O(n)
Space Complexity: O(h) where h is the height of the tree
