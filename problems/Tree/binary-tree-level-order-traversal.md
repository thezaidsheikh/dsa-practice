Prob: https://leetcode.com/problems/binary-tree-level-order-traversal/

Sol 1: Optimal approach - BFS queue

Steps:
1. Initialize an empty queue.
2. Initialize an empty list to store the levels.
3. If the root is not null, add it to the queue.
4. While the queue is not empty:
    a. Get the number of nodes in the current level (size).
    b. Initialize an empty list to store the current level's nodes.
    c. For each node in the current level:
        i. Remove the node from the queue.
        ii. Add the node's value to the current level's list.
        iii. If the node has a left child, add it to the queue.
        iv. If the node has a right child, add it to the queue.
    d. Add the current level's list to the list of levels.
5. Return the list of levels.
```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        List<List<Integer>> ls = new ArrayList<>();

        if (root != null) {
            queue.offer(root); // add root node into queue
        }

        while(!queue.isEmpty()) {
            List<Integer> list = new ArrayList<>();
            int size = queue.size();
            while(size-- != 0) {
                TreeNode t = queue.peek();
                list.add(queue.poll().val);
                if(t.left != null) queue.offer(t.left);
                if(t.right != null) queue.offer(t.right);
            }
            ls.add(list);
        }
        return ls;
    }
}
```