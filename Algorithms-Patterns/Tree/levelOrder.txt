# Level Order Traversal in Binary Tree (BFS)

## 1. Problem Understanding

**Level Order Traversal** means visiting the nodes of a binary tree **level by level** from **left to right**.

Example Tree:

        1
       / \
      2   3
     / \   \
    4   5   6

Level order traversal output:

[1, 2, 3, 4, 5, 6]

Level-wise representation:

Level 0 → [1]  
Level 1 → [2, 3]  
Level 2 → [4, 5, 6]

This traversal is naturally solved using **Breadth First Search (BFS)**.

---

# 2. Core Intuition

### Why BFS?

In BFS we process nodes **layer by layer**.

To achieve this, we use a **Queue (FIFO)**.

The queue ensures:
- The node inserted first is processed first.
- Nodes of the next level wait until the current level finishes.

---

## Step-by-Step Intuition

Example:

        1
       / \
      2   3
     / \   \
    4   5   6

### Step 1
Push root into queue.

Queue:
[1]

### Step 2
Process node 1

Add children:
2, 3

Queue:
[2, 3]

### Step 3
Process node 2

Add children:
4, 5

Queue:
[3, 4, 5]

### Step 4
Process node 3

Add child:
6

Queue:
[4, 5, 6]

### Step 5
Continue until queue becomes empty.

---

# 3. Standard Template (BFS for Trees)

This template works for **almost every level order problem**.

Core idea:

1. Push root into queue
2. While queue not empty
3. Get current level size
4. Process exactly those nodes
5. Add their children

Template:

```
Queue<TreeNode> queue
queue.add(root)

while queue not empty
    size = queue.size()

    for i = 0 → size
        node = queue.poll()

        process node

        if node.left
            queue.add(node.left)

        if node.right
            queue.add(node.right)
```

---

# 4. Java Implementation

## Tree Node Structure

```java
class TreeNode {

    int val;
    TreeNode left;
    TreeNode right;

    TreeNode(int val) {
        this.val = val;
    }
}
```

---

## Level Order Traversal (Return List of Levels)

```java
import java.util.*;

class Solution {

    public List<List<Integer>> levelOrder(TreeNode root) {

        List<List<Integer>> result = new ArrayList<>();

        if (root == null)
            return result;

        Queue<TreeNode> queue = new LinkedList<>();

        queue.offer(root);

        while (!queue.isEmpty()) {

            int size = queue.size();

            List<Integer> level = new ArrayList<>();

            for (int i = 0; i < size; i++) {

                TreeNode node = queue.poll();

                level.add(node.val);

                if (node.left != null)
                    queue.offer(node.left);

                if (node.right != null)
                    queue.offer(node.right);
            }

            result.add(level);
        }

        return result;
    }
}
```

---

# 5. Dry Run Example

Tree:

        1
       / \
      2   3
     / \   \
    4   5   6

Initial Queue:

[1]

Iteration 1

size = 1

level = [1]

Queue → [2,3]

Result:

[[1]]

---

Iteration 2

size = 2

level = [2,3]

Queue → [4,5,6]

Result:

[[1], [2,3]]

---

Iteration 3

size = 3

level = [4,5,6]

Queue → []

Final Result:

[[1], [2,3], [4,5,6]]

---

# 6. Time Complexity

Let:

n = number of nodes

Every node is:

- inserted into queue once
- removed once

Time Complexity:

O(n)

---

# 7. Space Complexity

Worst case:

Last level contains ~ n/2 nodes.

Queue stores them.

Space Complexity:

O(n)

---

# 8. Common Interview Variants

## 1. Binary Tree Level Order Traversal

Return nodes level by level.

Leetcode 102

Output:

[[1],[2,3],[4,5,6]]

---

## 2. Binary Tree Level Order Traversal II

Same traversal but **bottom-up order**.

Output:

[[4,5,6],[2,3],[1]]

Solution trick:

Reverse result at end.

```
Collections.reverse(result)
```

---

## 3. Zigzag Level Order Traversal

Levels alternate direction.

Example:

        1
       / \
      2   3
     / \   \
    4   5   6

Output:

[[1], [3,2], [4,5,6]]

Idea:

Reverse every alternate level.

Or insert at index.

---

### Java Example

```java
public List<List<Integer>> zigzagLevelOrder(TreeNode root) {

    List<List<Integer>> result = new ArrayList<>();

    if (root == null)
        return result;

    Queue<TreeNode> queue = new LinkedList<>();

    queue.offer(root);

    boolean reverse = false;

    while (!queue.isEmpty()) {

        int size = queue.size();

        List<Integer> level = new ArrayList<>();

        for (int i = 0; i < size; i++) {

            TreeNode node = queue.poll();

            level.add(node.val);

            if (node.left != null)
                queue.offer(node.left);

            if (node.right != null)
                queue.offer(node.right);
        }

        if (reverse)
            Collections.reverse(level);

        result.add(level);

        reverse = !reverse;
    }

    return result;
}
```

---

## 4. Right Side View

Return the **last node of each level**.

Example:

[1,3,6]

Trick:

Inside level loop:

```
if (i == size - 1)
    result.add(node.val)
```

---

## 5. Average of Levels in Binary Tree

Compute average of nodes per level.

---

## 6. Maximum Depth of Binary Tree (BFS version)

Depth = number of levels.

---

# 9. Key Interview Pattern

Whenever the problem mentions:

- "level"
- "each layer"
- "distance from root"
- "minimum steps"
- "shortest path"

Think:

BFS with Queue

---

# 10. Memorize This Template

```
Queue<TreeNode> queue = new LinkedList<>();

queue.offer(root)

while (!queue.isEmpty()) {

    int size = queue.size();

    for (int i = 0; i < size; i++) {

        TreeNode node = queue.poll();

        if (node.left != null)
            queue.offer(node.left);

        if (node.right != null)
            queue.offer(node.right);
    }
}
```

This **exact template solves ~80% of tree BFS problems**.

---

# 11. Quick Revision

Traversal Type

DFS Types
- Preorder
- Inorder
- Postorder

BFS Type
- Level Order Traversal

Data Structure Used

DFS → Stack / Recursion  
BFS → Queue

Complexity

Time → O(n)  
Space → O(n)

---

# 12. Interview Tip

Always remember the **3 golden steps**:

1. Queue initialization
2. Level size loop
3. Add children

If you remember these three, you can solve almost every **Level Order Tree problem** in interviews.