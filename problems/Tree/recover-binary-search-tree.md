Prob: https://leetcode.com/problems/recover-binary-search-tree/description/

Sol 1: Using DFS (Depth-First Search)

Solution Explanation:

1. Initialize three global variables `prev` to `null`, `first` to `null`, and `second` to `null`.
2. Recursively traverse the tree in-order.
3. For each node, check if the current node's value is less than the previous node's value. If it is less, and `first` is `null`, set `first` to the previous node. If `first` is not `null`, set `second` to the current node.
4. Update `prev` to the current node.
5. After traversal, swap the values of `first.val` and `second.val` to restore the BST.
6. Return the root of the tree.

Intuition: In-order traversal of a BST guarantees that the nodes are visited in ascending order. If we find two nodes whose values are out of order, we must swap them. We can ensure that we find the two nodes by remembering the previous node and the first out-of-order node found.

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
    private static TreeNode node1 = null;
    private static TreeNode node2 = null;

    public static void checkBST(TreeNode root) {
        if (root == null) {
            return;
        }

        checkBST(root.left);

        if (prev != null && root.val < prev.val) {
            if (node1 == null) {
                node1 = prev;
                node2 = root;
            } else {
                node2 = root;
            }
        }
        prev = root;

        checkBST(root.right);
    }

    public void recoverTree(TreeNode root) {
        node1 = null;
        node2 = null;
        prev = null;
        checkBST(root);

        // swap the values of the two misplaced nodes
        int temp = node1.val;
        node1.val = node2.val;
        node2.val = temp;
    }
}
```
