Prob: https://leetcode.com/problems/kth-largest-element-in-an-array/description/

Sol 1: Optimal - Using heap
1. To find kth largest element, we can use min heap.
2. Push all the elements in the heap.
3. Pop the elements from the heap k times.
4. Return the kth largest element.
```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        // Min-heap of size at most k
        PriorityQueue<Integer> pq = new PriorityQueue<>();

        for (int num : nums) {
            pq.offer(num);              // add current element
            if (pq.size() > k) {
                pq.poll();              // remove smallest if size > k
            }
        }

        // Now heap contains k largest elements,
        // root is the kth largest
        return pq.peek();
    }
}
```
Time Complexity: O(nlogk),
Space Complexity: O(k)
