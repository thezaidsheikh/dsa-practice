Prob: https://www.geeksforgeeks.org/problems/kth-smallest-element5635/1

Sol 1: Brute Force - Using sorting
1. Sort the entire array in ascending order.
2. After sorting, the Kth smallest element sits at index `k - 1` (K is 1-based).
3. Return `arr[k - 1]`.

```java
class Solution {
    public int kthSmallest(int[] arr, int k) {
        Arrays.sort(arr);
        return arr[k - 1];
    }
}
```
Time complexity - O(nlogn),
Space complexity - O(1)

Sol 2: Better - Using min heap
1. Insert all elements into a min heap.
2. Poll (remove the minimum) `k - 1` times to discard the k-1 smallest elements.
3. The next minimum, i.e. the heap top, is the Kth smallest element.

```java
class Solution {
    public int kthSmallest(int[] arr, int k) {
        PriorityQueue<Integer> pq = new PriorityQueue<>();

        for (int elem : arr) {
            pq.offer(elem);
        }

        for (int i = 1; i < k; i++) {
            pq.poll();
        }

        return pq.peek();
    }
}
```
Time complexity - O(n + klogn),
Space complexity - O(n)

Sol 3: Optimal - Using max heap of size k
# Intuition
We only care about the k smallest elements seen so far. A max heap capped at size k stores exactly those, and its root is the largest among them - which is the Kth smallest overall. Any incoming element larger than the root cannot be the answer, so it is safe to drop. The reverse comparator `(a, b) -> b - a` turns Java's default min heap into a max heap.

1. Create a max heap using `PriorityQueue<Integer> pq = new PriorityQueue<>((a, b) -> b - a)`.
2. Iterate through the array and offer each element into the heap.
3. If the heap size exceeds `k`, poll to remove the current largest element among them.
4. After one pass, the heap holds exactly the k smallest elements.
5. Return `pq.peek()`, the root = Kth smallest.

```java
class Solution {
    public int kthSmallest(int[] arr, int k) {
        PriorityQueue<Integer> pq = new PriorityQueue<>((a, b) -> b - a);

        for (int elem : arr) {
            pq.offer(elem);
            if (pq.size() > k) {
                pq.poll();
            }
        }

        return pq.peek();
    }
}
```
Time complexity - O(nlogk),
Space complexity - O(k)
