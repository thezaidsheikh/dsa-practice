Prob: https://leetcode.com/problems/task-scheduler/description/

Sol 1: Using Max heap
1. Store the frequency of each task in a map.
2. Create a map to store the next seat of each task.
3. Create a max heap to store the frequency of each task.
4. Store all chars with their frequencies in the heap.
5. Loop until the heap is empty.
6. Poll the top element from the heap.
7. If the expected seat is greater than the current seat, then add the top element to the pending list.
8. If the frequency of the top element is greater than 1, then decrement the frequency and add the top element to the heap.
9. Update the seat of the top element in the seat map.
10. Add all the pending elements to the heap.
11. Return the seat.
```java
class Solution {
    public int leastInterval(char[] tasks, int n) {
        // 1. Count frequencies
        Map<Character, Integer> freq = new HashMap<>();
        Map<Character, Integer> seatMap = new HashMap<>();
        for (char c : tasks) {
            freq.put(c, freq.getOrDefault(c, 0) + 1);
            seatMap.put(c,1);
        }

        // 2. Max-heap by frequency: each element is int[]{freq, char}
        PriorityQueue<int[]> pq = new PriorityQueue<>(
            (a, b) -> b[0] - a[0]   // descending by frequency
        );

        // 3. Store all chars with their frequencies in the heap
        for (Map.Entry<Character, Integer> e : freq.entrySet()) {
            pq.offer(new int[]{e.getValue(), e.getKey()});
        }

        int seat = 1;
        while(!pq.isEmpty()) {
            List<int[]> pending_ls = new ArrayList<>();
            while(!pq.isEmpty()) {
                int[] top = pq.poll();
                int expected_seat = seatMap.getOrDefault((char)top[1], 1);
                if(expected_seat > seat) {
                    pending_ls.add(top);
                    continue;
                }
                if(top[0] > 1) {
                    --top[0];
                    pq.offer(top);
                }
                seatMap.put((char)top[1],seat + n+1);
                break;
            }
            for(int[] pair: pending_ls) {
                pq.offer(pair);
            }
            seat++;
        }
        return --seat;
    }
}
```
Time complexity = O(NlogN)
Space complexity = O(N)