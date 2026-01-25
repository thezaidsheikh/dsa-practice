Prob: https://www.geeksforgeeks.org/problems/kth-smallest-element5635/1

Sol 1: Optimal - Using heap
1. To find kth smallest element, we can use max heap.
2. In max heap we have top element as the largest element.
3. We will add elements in the heap and if size of the heap is greater then k, then we will remove the top element.
4. At the end, the top element will be the kth smallest element.
5. Return the top element.
```java
class Solution {
    public int kthSmallest(int[] arr, int k) {
        PriorityQueue<Integer> pq = new PriorityQueue<>((a, b) -> b - a);

        for(int elem:arr) {
            pq.offer(elem);
            if(pq.size() > k) {
                pq.poll();
            }
        }
        return pq.peek();
    }
}
```
Time Complexity: O(nlogk),
Space Complexity: O(k)
