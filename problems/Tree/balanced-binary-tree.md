Prob: https://leetcode.com/problems/balanced-binary-tree/

Sol 1: Optimal approach - Using DFS (Depth First Search)

1. Start from the root node.
2. If the current node is null, return true.
3. If the current node is a leaf node (both left and right children are null), return true.
4. If the current node has only one child, return true if the child is balanced and false otherwise.
5. If the current node has two children, check if both children are balanced. If both children are balanced and the absolute difference between their heights is less than or equal to 1, return true. Otherwise, return false.
6. Repeat steps 2-5 until the balanced property is determined for the entire tree.

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
    // 1. Instance variable (field), NOT static, NOT local
    private boolean ans;

    // 2. Recursive helper: instance method
    private int rec(TreeNode root) {
        if (root == null) return 0;

        int lh = rec(root.left);
        int rh = rec(root.right);

        if (Math.abs(lh - rh) > 1) {
            ans = false; // 3. update the field
        }

        return 1 + Math.max(lh, rh);
    }

    public boolean isBalanced(TreeNode root) {
        ans = true;       // reset flag for this call
        rec(root);
        return ans;
    }
}
```

Time Complexity: O(n)
Space Complexity: O(n)
