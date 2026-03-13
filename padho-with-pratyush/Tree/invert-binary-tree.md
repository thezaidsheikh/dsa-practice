Prob: https://leetcode.com/problems/invert-binary-tree/

Sol 1: Optimal approach - DFS (Recursive)

1. Initialize a new TreeNode called `result` with the value of the `root` TreeNode.
2. If the `root` TreeNode is null, return `result`.
3. Swap the `left` and `right` TreeNodes of the `root` TreeNode.
4. Recursively call the `invertTree` function on the `left` and `right` TreeNodes of the `root` TreeNode.
5. Return the `result` TreeNode.

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
    private void swapChildren(TreeNode node) {
        TreeNode temp = node.left;
        node.left = node.right;
        node.right = temp;
    }
    public TreeNode invertTree(TreeNode root) {
        if (root == null) {
            return null;
        }

        swapChildren(root);
        invertTree(root.left);
        invertTree(root.right);

        return root;
    }
}
```

Time Complexity: O(N)
Space Complexity: O(H) where H is the height of the tree
