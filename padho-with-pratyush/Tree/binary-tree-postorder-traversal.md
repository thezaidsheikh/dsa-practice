Prob: https://leetcode.com/problems/binary-tree-postorder-traversal/description/

Sol 1: Optimal Approach - Using Tree
1. Post means first left children value, then right childer value, then root value;
```java
class Solution {
    public static void rec(TreeNode root, List<Integer> ls) {
        if(root == null) return;
        rec(root.left,ls);
        rec(root.right,ls);
        ls.add(root.val);
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