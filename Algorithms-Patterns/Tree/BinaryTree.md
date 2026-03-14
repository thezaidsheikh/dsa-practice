# Binary Tree — Complete Study Notes

---

1. Definition

---

A Binary Tree is a hierarchical data structure where each node can have **at most two children**.

Children are called: - Left Child - Right Child

Structure:

            Root
           /    \
       Left     Right

Each node contains: - Data - Reference to left child - Reference to right child

---

2. Binary Tree Node Structure (Java)

---

class TreeNode {

    int val;
    TreeNode left;
    TreeNode right;

    TreeNode(int val) {
        this.val = val;
        this.left = null;
        this.right = null;
    }

}

---

3. Basic Terminology

---

Root
Top node of the tree.

Parent
Node that has children.

Child
Node connected below a parent.

Leaf Node
Node with no children.

Height
Number of edges in the longest path from root to leaf.

Depth
Distance from root to a node.

Level
Nodes that exist at the same depth.

Subtree
Tree formed from any node and its descendants.

---

4. Types of Binary Trees

---

4.1 Full Binary Tree

Every node has either: - 0 children - 2 children

Example:

        1
       / \
      2   3
         / \
        4   5

---

4.2 Complete Binary Tree

All levels are completely filled except possibly the last level.
The last level is filled from left to right.

Example:

        1
       / \
      2   3
     / \  /
    4  5 6

Used in:
Heap data structure

---

4.3 Perfect Binary Tree

All levels are completely filled.

Example:

        1
       / \
      2   3
     / \ / \
    4  5 6  7

Property:

Number of nodes = 2^h - 1

---

4.4 Balanced Binary Tree

Height difference between left and right subtree ≤ 1.

Examples:
AVL Tree
Red Black Tree

Benefit:
Operations become O(log n)

---

4.5 Degenerate Binary Tree

Each parent node has only one child.

Example:

1
\
 2
\
 3
\
 4

Behaves like a Linked List.

---

4.6 Skewed Binary Tree

Left Skewed:

        1
       /
      2
     /
    3

Right Skewed:

1
\
 2
\
 3

---

5. Tree Traversal

---

Traversal means visiting every node in the tree.

Two major categories:

    1. DFS (Depth First Search)
    2. BFS (Breadth First Search)

---

6. DFS (Depth First Search)

---

DFS explores one branch completely before moving to another.

Uses: - Recursion - Stack

Three types:

    1. Preorder
    2. Inorder
    3. Postorder

Example Tree:

        1
       / \
      2   3
     / \
    4   5

---

7. DFS Traversals

---

7.1 Preorder Traversal

Order:
Root → Left → Right

Output:
1 2 4 5 3

Java Code:

void preorder(TreeNode root) {

    if (root == null)
        return;

    System.out.print(root.val + " ");

    preorder(root.left);
    preorder(root.right);

}

Time Complexity:
O(n)

Space Complexity:
O(h)

---

7.2 Inorder Traversal

Order:
Left → Root → Right

Output:
4 2 5 1 3

Java Code:

void inorder(TreeNode root) {

    if (root == null)
        return;

    inorder(root.left);

    System.out.print(root.val + " ");

    inorder(root.right);

}

Special Case:
In Binary Search Tree, inorder traversal gives sorted output.

---

7.3 Postorder Traversal

Order:
Left → Right → Root

Output:
4 5 2 3 1

Java Code:

void postorder(TreeNode root) {

    if (root == null)
        return;

    postorder(root.left);
    postorder(root.right);

    System.out.print(root.val + " ");

}

---

8. DFS Template

---

DFS(node)

if node == null
return

process node

DFS(node.left)

DFS(node.right)

---

9. BFS (Breadth First Search)

---

BFS explores the tree level by level.

Uses:
Queue (FIFO)

Applications: - Level order traversal - Shortest path - Minimum depth

Example Tree:

        1
       / \
      2   3
     / \   \
    4   5   6

Traversal Output:
1 2 3 4 5 6

---

10. BFS Java Implementation

---

import java.util.\*;

void levelOrder(TreeNode root) {

    if (root == null)
        return;

    Queue<TreeNode> queue = new LinkedList<>();

    queue.offer(root);

    while (!queue.isEmpty()) {

        TreeNode node = queue.poll();

        System.out.print(node.val + " ");

        if (node.left != null)
            queue.offer(node.left);

        if (node.right != null)
            queue.offer(node.right);
    }

}

---

11. BFS Template

---

Queue q

q.add(root)

while q not empty

    node = q.poll()

    process node

    if node.left
        q.add(node.left)

    if node.right
        q.add(node.right)

---

12. DFS vs BFS

---

DFS

Strategy:
Go deep first

Data Structure:
Stack / Recursion

Memory:
Less

Typical Use:
Path problems
Height problems

BFS

Strategy:
Level by level

Data Structure:
Queue

Memory:
More

Typical Use:
Shortest path
Level problems

---

13. Time and Space Complexity

---

Let n = number of nodes.

DFS

Time Complexity:
O(n)

Space Complexity:
O(h)

BFS

Time Complexity:
O(n)

Space Complexity:
O(n)

---

14. Common Binary Tree Interview Problems

---

Basic

    Maximum Depth
    Minimum Depth
    Count Nodes
    Sum of Nodes

Traversal Problems

    Level Order Traversal
    Zigzag Traversal
    Vertical Order Traversal

Structural Problems

    Symmetric Tree
    Same Tree
    Subtree of Another Tree

Path Problems

    Path Sum
    Root to Leaf Paths
    Maximum Path Sum

Construction Problems

    Build Tree from Preorder + Inorder
    Build Tree from Inorder + Postorder

Advanced Problems

    Lowest Common Ancestor
    Diameter of Binary Tree
    Serialize and Deserialize Tree

---

15. Interview Recognition Patterns

---

Use BFS when problem mentions:

    - Level
    - Distance
    - Minimum steps
    - Layer by layer

Use DFS when problem mentions:

    - Height
    - Path
    - Depth
    - Recursive structure

---

16. Master Templates

---

DFS Template

solve(node)

if node == null
return

solve(node.left)

solve(node.right)

BFS Template

Queue q

q.add(root)

while q not empty

    size = q.size()

    for i from 0 to size

        node = q.poll()

        process node

        add children

---

17. Quick Revision

---

Binary Tree
Node with at most two children

Traversals

    DFS
        Preorder
        Inorder
        Postorder

    BFS
        Level Order

Data Structures

    DFS → Stack / Recursion
    BFS → Queue

Time Complexity

    O(n)

Space Complexity

    DFS → O(h)
    BFS → O(n)

---

## End of Binary Tree Notes
