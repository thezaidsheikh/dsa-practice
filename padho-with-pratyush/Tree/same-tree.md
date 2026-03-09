Prob: https://leetcode.com/problems/same-tree/description/

Sol 1: Optimal approach - Using recursion
Steps:
1. Check if both trees are null, if true, return true.
2. Check if both trees are not null, if true, proceed.
3. Check if root values are equal, if true, proceed.
4. Recursively call isSameTree() for left and right sub-trees.
5. Return the result of recursive calls.
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
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if(p == null && q == null) return true;
        if(p == null || q == null) return false;
        if(p.val != q.val) return false;
        boolean r1 = isSameTree(p.left, q.left);
        if(!r1) return r1;
        boolean r2 = isSameTree(p.right, q.right);
        if(!r2) return r2;
        return true;
    }
}
```
Time complexity - O(N),
Space complexity - O(H)