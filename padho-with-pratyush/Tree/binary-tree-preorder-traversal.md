Prob: https://leetcode.com/problems/binary-tree-preorder-traversal/description/

Sol 1: Optimal approach - Using Tree
1. Preorder means first root value, then left childer value, then right children value;
```java
class Solution {
    public static void rec(TreeNode root, List<Integer> ls) {
        if(root == null) return;
        ls.add(root.val);
        rec(root.left, ls);
        rec(root.right, ls);
    }
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> ls = new ArrayList<>();
        rec(root,ls);
        return ls;
    }
}
```
Time complexity - O(N),
Space complexity - O(N)