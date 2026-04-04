Prob: https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/

Sol 1: Optimal

### Solution Explanation

The intuition behind this solution is that, for a given preorder and inorder traversal, we can construct the binary tree by making use of the preorder traversal. Here's a step-by-step explanation:

1. **Preprocess Inorder Array**: We process the inorder array to create a map where the key is the value of the node and the value is the index of that node in the inorder array. This is done in O(n) time.

2. **Build Subtree**: We recursively build the subtree using the preorder and inorder traversal. Given the range of inorder indices, we pick the root of the subtree to be the first element in the preorder array. We then find the index of the root in the inorder array and construct the left and right subtrees using the remaining inorder indices. This step is also done in O(n) time.

3. **Construct Binary Tree**: Finally, we construct the entire binary tree by calling the build function with the entire range of inorder indices. This step is also done in O(n) time.

Overall, the time complexity of this solution is O(n) where n is the size of the input arrays.

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
    // Global index to track position in preorder
    private int preIndex = 0;
    // Map to store value -> index mapping for inorder array
    private Map<Integer, Integer> inIndexMap;

    // Build subtree using inorder range [inLeft, inRight]
    private TreeNode build(int[] preorder, int inLeft, int inRight) {
        // Base case: no elements to construct the subtree
        if (inLeft > inRight) {
            return null;
        }

        // preorder[preIndex] is the root for this subtree
        int rootVal = preorder[preIndex++];
        TreeNode root = new TreeNode(rootVal);

        // Find the root position in inorder
        int inRootIndex = inIndexMap.get(rootVal);

        // Elements in inorder[inLeft .. inRootIndex-1] belong to left subtree
        root.left = build(preorder, inLeft, inRootIndex - 1);
        // Elements in inorder[inRootIndex+1 .. inRight] belong to right subtree
        root.right = build(preorder, inRootIndex + 1, inRight);

        return root;
    }

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        int n = preorder.length;
        inIndexMap = new HashMap<>();

        // Fill the map so we can find the index of any value in O(1)
        for (int i = 0; i < n; i++) {
            inIndexMap.put(inorder[i], i);
        }

        // Build the tree from the whole range of inorder: [0, n - 1]
        return build(preorder, 0, n - 1);
    }
}
```

Time Complexity: O(n)
Space Complexity: O(n)
