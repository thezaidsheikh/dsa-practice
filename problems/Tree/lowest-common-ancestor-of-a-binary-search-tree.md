Prob: https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/

Sol 1: Optimal approach - Using BST property

1. Start from the root node.
2. If the current node is null, return null.
3. If the current node's value is greater than both p and q values, search in the left subtree.
4. If the current node's value is less than both p and q values, search in the right subtree.
5. If the current node's value is between p and q values, return the current node.
6. Repeat steps 2-5 until the LCA is found.

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */

class Solution {
    public static TreeNode ans = null;
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {

        if(root == null) return ans;
        if(p.val < root.val && q.val < root.val) lowestCommonAncestor(root.left, p, q);
        else if(p.val > root.val && q.val > root.val) lowestCommonAncestor(root.right, p, q);
        else if((p.val <= root.val && q.val >= root.val) || (p.val >= root.val && q.val <= root.val)) {
            ans = root;
        }

        return ans;
    }
}
```

Time Complexity: O(log n)
Space Complexity: O(log n)
