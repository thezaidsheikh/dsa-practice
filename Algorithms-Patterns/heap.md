# Heap

## Definition

A heap is a complete binary tree that satisfies a heap-order property.

There are two main types:

- **Min Heap**: every parent is smaller than or equal to its children, so the minimum element is at the root.
- **Max Heap**: every parent is greater than or equal to its children, so the maximum element is at the root.

In DSA problems, heaps are usually implemented using an array or a priority queue.

## Why Heap Is Important

Heap is one of the most useful data structures when you repeatedly need access to:

- the smallest element
- the largest element
- the next highest-priority element
- the top `k` elements
- a running minimum or maximum while processing data

Heaps are important because they let us do these operations much faster than keeping a normal unsorted array or list.

For example:

- finding the smallest element in an unsorted array is `O(n)`
- getting the top element from a heap is `O(1)`
- inserting into a sorted array can be `O(n)`
- inserting into a heap is `O(log n)`

This is why heap appears frequently in:

- top `k` problems
- scheduling problems
- merging sorted lists/arrays
- graph algorithms like Dijkstra and Prim
- streaming data problems
- median / kth largest / kth smallest questions

## Heap Properties

### 1. Complete Binary Tree

A heap is a complete binary tree, which means:

- every level is fully filled except possibly the last
- the last level is filled from left to right

Because of this property, a heap can be stored efficiently in an array.

If an element is at index `i`:

- left child = `2 * i + 1`
- right child = `2 * i + 2`
- parent = `(i - 1) / 2`

These formulas are for `0`-based indexing.

### 2. Heap Order Property

Only parent-child ordering is guaranteed.

That means:

- in a min heap, parent <= children
- in a max heap, parent >= children

Important:

- the entire array is **not** sorted
- only the root is guaranteed to be min or max

## Min Heap vs Max Heap

### Min Heap

Useful when the smallest element should come out first.

Examples:

- kth largest element using a min heap of size `k`
- merge `k` sorted lists
- Dijkstra's shortest path

Example min heap:

```text
        2
      /   \
     5     8
    / \   / \
   9  12 10 15
```

Array representation:

```text
[2, 5, 8, 9, 12, 10, 15]
```

### Max Heap

Useful when the largest element should come out first.

Examples:

- repeatedly taking the maximum
- heap sort
- finding the kth smallest element using a max heap of size `k`

Example max heap:

```text
        20
      /    \
    15      18
   /  \    /  \
  10   8  12   5
```

Array representation:

```text
[20, 15, 18, 10, 8, 12, 5]
```

## How Heap Works

The two core restructuring operations are:

- **sift up** / **bubble up**
- **sift down** / **heapify down**

### Sift Up

Used after inserting a new element at the end.

Steps:

1. Insert the new element at the last position.
2. Compare it with its parent.
3. If heap order is violated, swap.
4. Continue until the heap property is restored.

### Sift Down

Used after removing the root.

Steps:

1. Replace the root with the last element.
2. Remove the last element.
3. Compare the new root with its children.
4. Swap with the correct child if heap order is violated.
5. Continue until the heap property is restored.

## Important Heap Operations

### 1. Peek / Top

Returns the root element.

- min heap -> minimum
- max heap -> maximum

Time Complexity: `O(1)`
Space Complexity: `O(1)`

### 2. Insert / Offer / Push

Insert the new element at the end, then sift up.

Time Complexity: `O(log n)`
Space Complexity: `O(1)` auxiliary

### 3. Remove Top / Poll / Pop

Remove the root, move the last element to the root, then sift down.

Time Complexity: `O(log n)`
Space Complexity: `O(1)` auxiliary

### 4. Build Heap

Convert an unsorted array into a heap.

Two ways:

- repeated insertion -> `O(n log n)`
- bottom-up heapify -> `O(n)`

The optimal build-heap method is bottom-up heapify.

Time Complexity: `O(n)`
Space Complexity: `O(1)` if done in-place

### 5. Heapify

Heapify means restoring the heap property from a node downward.

This is used in:

- build heap
- remove top
- heap sort

Time Complexity: `O(log n)`
Space Complexity: `O(1)` auxiliary

## Operation Complexity Table

| Operation | Time | Space |
| --- | --- | --- |
| Peek top element | `O(1)` | `O(1)` |
| Insert element | `O(log n)` | `O(1)` auxiliary |
| Remove top element | `O(log n)` | `O(1)` auxiliary |
| Build heap from array | `O(n)` | `O(1)` in-place |
| Heapify one node | `O(log n)` | `O(1)` auxiliary |
| Search for arbitrary value | `O(n)` | `O(1)` |

Important note:

- heaps are excellent for root access
- heaps are poor for arbitrary search because heap order is partial, not fully sorted

## Useful Methods In Java PriorityQueue

In Java, heap problems are commonly solved using `PriorityQueue`.

By default:

- `PriorityQueue<Integer>` is a **min heap**

To create a max heap:

- use `Collections.reverseOrder()`

### Min Heap Example

```java
import java.util.PriorityQueue;

PriorityQueue<Integer> minHeap = new PriorityQueue<>();

minHeap.offer(10);
minHeap.offer(4);
minHeap.offer(7);
minHeap.offer(2);

System.out.println(minHeap.peek()); // 2
System.out.println(minHeap.poll()); // 2
System.out.println(minHeap.peek()); // 4
```

### Max Heap Example

```java
import java.util.Collections;
import java.util.PriorityQueue;

PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());

maxHeap.offer(10);
maxHeap.offer(4);
maxHeap.offer(7);
maxHeap.offer(20);

System.out.println(maxHeap.peek()); // 20
System.out.println(maxHeap.poll()); // 20
System.out.println(maxHeap.peek()); // 10
```

### Important Java Heap Methods

| Method | Meaning | Time | Space |
| --- | --- | --- | --- |
| `offer(x)` | insert element | `O(log n)` | `O(1)` auxiliary |
| `add(x)` | insert element | `O(log n)` | `O(1)` auxiliary |
| `peek()` | read top element | `O(1)` | `O(1)` |
| `poll()` | remove and return top | `O(log n)` | `O(1)` auxiliary |
| `remove()` | remove top element | `O(log n)` | `O(1)` auxiliary |
| `size()` | number of elements | `O(1)` | `O(1)` |
| `isEmpty()` | check emptiness | `O(1)` | `O(1)` |

Notes:

- `peek()` returns `null` if the heap is empty
- `poll()` returns `null` if the heap is empty
- `remove()` throws an exception if the heap is empty
- `offer()` and `add()` are both used for insertion, but `offer()` is usually preferred for queues

## Manual Heap Example Using Array Idea

Suppose we insert into a min heap in this order:

```text
10, 4, 15, 2
```

### Step 1: insert 10

```text
[10]
```

### Step 2: insert 4

Insert at end:

```text
[10, 4]
```

Now sift up:

```text
[4, 10]
```

### Step 3: insert 15

```text
[4, 10, 15]
```

No change needed because `15 >= 4`.

### Step 4: insert 2

Insert at end:

```text
[4, 10, 15, 2]
```

Sift up with parent `10`:

```text
[4, 2, 15, 10]
```

Sift up again with parent `4`:

```text
[2, 4, 15, 10]
```

Now heap property is restored.

## Example: Remove From Min Heap

Current heap:

```text
[2, 4, 15, 10]
```

Remove top:

1. Remove `2`
2. Move last element `10` to root
3. Heap becomes `[10, 4, 15]`
4. Compare `10` with children `4` and `15`
5. Swap with smaller child `4`

Final heap:

```text
[4, 10, 15]
```

## Common Problem Patterns Using Heap

### 1. Top K Elements

Use a min heap of size `k` when finding:

- kth largest element
- top `k` frequent elements
- k closest points

Idea:

- keep only the best `k` candidates in the heap
- if heap size exceeds `k`, remove the smallest / least useful element

This keeps complexity better than sorting the entire input in many cases.

### 2. Repeated Min or Max Access

Use heap when you repeatedly need:

- smallest task
- largest value
- next available resource

This is common in scheduling and greedy problems.

### 3. Merge K Sorted Structures

Use a min heap to merge:

- `k` sorted arrays
- `k` sorted linked lists

At each step:

- take the smallest current element
- insert the next element from the same source

### 4. Running Median

Use two heaps:

- max heap for the smaller half
- min heap for the larger half

This allows efficient median maintenance as numbers arrive one by one.

### 5. Graph Algorithms

Priority queues are fundamental in:

- Dijkstra's algorithm
- Prim's algorithm

Because we repeatedly choose the node or edge with minimum current cost.

## Differences From Similar Structures

### Heap vs Sorted Array

- sorted array gives fast access to min/max, but insertion is expensive
- heap gives fast insertion and fast top removal, but is not fully sorted

### Heap vs BST

- balanced BST supports ordered operations more flexibly
- heap is usually simpler and best when only top-priority access matters

### Heap vs HashMap

- hash map is for key lookup
- heap is for priority ordering

Often both are used together, such as in top frequency problems.

## When To Think About Heap

Ask these questions:

- Do I repeatedly need the smallest or largest element?
- Do I need only the top `k` elements instead of fully sorting everything?
- Does the problem mention priorities, tasks, scheduling, or closest/farthest items?
- Am I processing a stream of values where order by priority matters?
- Do I need efficient repeated extraction of min or max?

If the answer is yes, heap is a strong candidate.

## Common Mistakes

- assuming the whole heap is sorted
- using a heap when a single sort is simpler and sufficient
- forgetting whether the problem needs a min heap or max heap
- forgetting to limit heap size to `k` in top `k` problems
- counting heap search as `O(log n)` for arbitrary values, which is incorrect

## Example Problems

- Kth Largest Element in an Array
- Top K Frequent Elements
- Merge K Sorted Lists
- Find Median from Data Stream
- Task Scheduler
- IPO
- K Closest Points to Origin
- Dijkstra's Shortest Path

## Complexity Summary

- Access top element: `O(1)`
- Insert: `O(log n)`
- Remove top: `O(log n)`
- Build heap: `O(n)`
- Search arbitrary element: `O(n)`

## Key Insight

Heap is the go-to data structure when full sorting is unnecessary and all you really need is fast access to the current highest-priority element.
