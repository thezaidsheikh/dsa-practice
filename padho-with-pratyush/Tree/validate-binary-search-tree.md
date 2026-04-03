Prob: https://leetcode.com/problems/validate-binary-search-tree/description/

Sol 1: Using DFS (Depth-First Search)

Solution Explanation:

1. Initialize a global variable `prev` to `null` and `res` to `true`.
2. Recursively traverse the tree in-order.
3. For each node, check if the current node's value is less than or equal to the previous node's value. If it is not, set `res` to `false`.
4. Update `prev` to the current node.
5. If `res` is still `true` at the end of traversal, the tree is a valid BST.
6. Return `res`.

Intuition: In-order traversal of a BST guarantees that the nodes are visited in ascending order. If at any point we find a node whose value is less than the previous node's value, it means the tree is not a BST.

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
    private static TreeNode prev = null;
    private static boolean res = true;

    public static void checkBST(TreeNode root) {
        if (root == null) {
            return;
        }

        checkBST(root.left);
        if (prev == null) {
            prev = root;
        } else {
            if (root.val <= prev.val) {
                res = false;
            }
            prev = root;
        }

        if(res == false) return;
        checkBST(root.right);
        return;
    }

    public boolean isValidBST(TreeNode root) {
        res = true;
        prev = null;
        checkBST(root);
        return res;
    }
}
```
