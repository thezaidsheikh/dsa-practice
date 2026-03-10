Prob: https://leetcode.com/problems/symmetric-tree/description/

Sol 1: Optimal approach - Using recursion

Steps:
1. Check if root is null, if true, return true.
2. Check if root is not null, if true, proceed.
3. Check if root.left and root.right are not null, if true, proceed.
4. Check if root.left.val and root.right.val are equal, if true, proceed.
5. Recursively call isSymmetric() for left and right sub-trees.
6. Return the result of recursive calls.
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
    public static boolean checkSymmetric(TreeNode t1, TreeNode t2) {
        if(t1 == null && t2 == null) return true;
        else if(t1 == null || t2 == null) return false;

        if(t1.val != t2.val) return false;
        boolean res1 = checkSymmetric(t1.left, t2.right);
        if(!res1) return false;
        boolean res2 = checkSymmetric(t1.right, t2.left);
        if(!res2) return false;
        return true;
    }
    public boolean isSymmetric(TreeNode root) {
        return checkSymmetric(root.left, root.right);
    }
}
```
Time complexity - O(N)
Space complexity - O(H)