Prob: https://leetcode.com/problems/search-in-a-binary-search-tree/

Sol 1: Optimal approach - Using BST property

1. Start from the root node.
2. If the current node is null, return null.
3. If the current node's value is equal to the target value, return the current node.
4. If the current node's value is greater than the target value, search in the left subtree.
5. If the current node's value is less than the target value, search in the right subtree.
6. Repeat steps 2-5 until the target value is found or the current node is null.

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
    public TreeNode searchBST(TreeNode root, int val) {
        if(root == null) return null;
        if(root.val == val) return root;

        if(val > root.val) return searchBST(root.right,val);
        else return searchBST(root.left,val);
    }
}
```

Time Complexity: O(log n)
Space Complexity: O(log n)
