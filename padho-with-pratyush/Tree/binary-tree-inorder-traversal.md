Prob: https://leetcode.com/problems/binary-tree-inorder-traversal/description/

Sol 1: Optimal Approach - Using Tree
1. In order means first left children value, then root value, then right children value;
```java
class Solution {
    public static void rec(TreeNode root, List<Integer> ls) {
        if(root == null) return;
        rec(root.left,ls);
        ls.add(root.val);
        rec(root.right,ls);
    }
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> ls = new ArrayList<>();
        rec(root,ls);
        return ls;
    }
}
```
Time Complexity - O(N),
Space Complexity - O(N)