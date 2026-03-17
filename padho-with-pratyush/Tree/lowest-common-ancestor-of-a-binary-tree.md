Prob: https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/

Sol 1: Optimal approach - Using DFS

1. Start a DFS from the root of the tree towards the p and q nodes.
2. Keep track of three variables: `pFound`, `qFound`, and `lca`.
   - `pFound` is a boolean that indicates if the node p has been found in the current subtree.
   - `qFound` is a boolean that indicates if the node q has been found in the current subtree.
   - `lca` is a reference to the lowest common ancestor of the p and q nodes (initialized to null).
3. If the current node is equal to p or q, set the corresponding boolean variable to true.
4. If both p and q have been found, then the current node is the lowest common ancestor (LCA). Set `lca` to the current node.
5. If only one of the p and q has been found, and the other has not been found yet, then the current node is the LCA. Set `lca` to the current node.
6. If none of the p and q have been found in the current subtree, then continue the DFS in the left and right subtrees.
7. After visiting all nodes in the tree, the `lca` variable will contain the lowest common ancestor of the p and q nodes.

Note: The key insight behind this approach is that the LCA is the last node we visited that has both p and q as children. This is because if we visit a node that has both p and q as children, then we know that the LCA cannot be any of its descendants, since both p and q have been found in the current subtree.

Here is the Java code implementation of this approach:

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
    private static TreeNode ans = null;
    public static int fun(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null) return 0;
        int self = 0;

        int left = fun(root.left, p, q);
        int right = fun(root.right, p, q);
        if(root.val == p.val || root.val == q.val) self = 1;

        int sum = left + right + self;
        if(ans == null && sum >= 2) ans = root;
        return sum;
    }
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        ans = null;
        fun(root, p, q);
        return ans;
    }
}
```

Time Complexity: O(n)
Space Complexity: O(h) where h is the height of the tree
