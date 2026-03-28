Prob: https://leetcode.com/problems/path-sum-ii/description/

Sol 1: Using DFS (Depth-First Search)

Sol 1: Using DFS (Depth-First Search)

1. Start from root node.
2. If the current node is null, return.
3. Add the value of the current node to the sum.
4. Add the value of the current node to the list.
5. If the current node is a leaf node (both left and right children are null), check if the sum is equal to the target. If yes, add a copy of the list to the result.
6. If the current node is not a leaf node, recursively call checkSum() on the left and right children.
7. Remove the value of the current node from the sum.
8. Remove the value of the current node from the list.

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
    private static List<List<Integer>> res = new ArrayList<>();

    public static void checkSum(TreeNode root, int sum, List<Integer> ls, int target) {
        if (root == null)
            return;

        sum += root.val;
        ls.add(root.val);

        if (root.left == null && root.right == null) {
            if (target == sum) {
                res.add(new ArrayList<>(ls));
            }

        } else {
            checkSum(root.left, sum, ls, target);
            checkSum(root.right, sum, ls, target);
        }

        ls.remove(ls.size() - 1);

    }

    public List<List<Integer>> pathSum(TreeNode root, int targetSum) {
        res = new ArrayList<>();
        checkSum(root, 0, new ArrayList<>(), targetSum);
        return res;
    }
}
```
