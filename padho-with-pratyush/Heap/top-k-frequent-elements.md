Prob: https://leetcode.com/problems/top-k-frequent-elements/description/

Sol 1: Optimal - Using min heap and hashmap
1. Insert element and its freq into hashmap
2. Create a min heap of size k as we have to find k elements
3. Insert k elements and its freq into heap.
4. After inserting k elements check the top element if it is less then current freq then it is obvious that it will be less than other freq on heap.
5. If it is bigger than remove the top element and insert this new element and its freq.
6. Ths final answer will be the remaining k elements in heap
```java
class Solution {
    
    public int[] topKFrequent(int[] nums, int k) {
        int n = nums.length;
        Map<Integer,Integer> mp = new HashMap<>();

        // Min Heap
        PriorityQueue<int[]> pq = new PriorityQueue<>(
            (a, b) -> a[1] - b[1]
        );

        // Inserting element and its frequency in hashmap
        for(int i = 0; i < n; i++) {
            mp.put(nums[i],mp.getOrDefault(nums[i],0)+1);
        }

        for (Map.Entry<Integer, Integer> entry : mp.entrySet()) {
            if(pq.size() < k) {
                pq.offer(new int[]{entry.getKey(), entry.getValue()});
                continue;
            }

            int[] top = pq.peek();
            if(entry.getValue() < top[1]) continue;
            pq.poll();
            pq.offer(new int[]{entry.getKey(), entry.getValue()});
        }

        // Step 4: Extract result
        int[] result = new int[k];
        int idx = 0;

        while (!pq.isEmpty()) {
            result[idx++] = pq.poll()[0];
        }

        return result;
    }
}
```