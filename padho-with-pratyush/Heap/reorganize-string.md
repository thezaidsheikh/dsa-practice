Prob: https://leetcode.com/problems/reorganize-string/description/

Sol 1: Optimal approach - Using max heap and Greedy approach
1. Create a max heap to store the characters and their frequencies.
2. Keep track of the count of current string.
3. While the count is less than (length of string / 2) + 1,
      a. Pop the maximum element from the heap.
      b. Append the character to the current string.
      c. Decrement the frequency of the popped character in the heap.
      c. If the frequency of the popped character becomes 0, remove it from the heap.
      d. Increment the count.
4. If the count is not equal to (length of string / 2) + 1, return an empty string as it is not possible to reorganize the string.
5. Repeat the steps 3 to 4 until the heap is empty.
6. Return the current string.
```java
class Solution {
    public String reorganizeString(String s) {
        // 1. Count frequencies
        Map<Character, Integer> freq = new HashMap<>();
        for (char c : s.toCharArray()) {
            freq.put(c, freq.getOrDefault(c, 0) + 1);
        }

        // Optional: quick impossibility check
        int n = s.length();
        int maxFreq = 0;
        for (int f : freq.values()) {
            maxFreq = Math.max(maxFreq, f);
        }
        if (maxFreq > (n + 1) / 2) return "";

        // 2. Max-heap by frequency: each element is int[]{freq, char}
        PriorityQueue<int[]> pq = new PriorityQueue<>(
            (a, b) -> b[0] - a[0]   // descending by frequency
        );

        // 3. Store all chars with their frequencies in the heap
        for (Map.Entry<Character, Integer> e : freq.entrySet()) {
            pq.offer(new int[]{e.getValue(), e.getKey()});
        }

        StringBuilder sb = new StringBuilder();
        int pos = 0;
        while(!pq.isEmpty()) {
            int[] top = pq.poll();
            if(pos == 0 || sb.charAt(pos - 1) != top[1]) {
                sb.append((char)top[1]);
                pos++;
                if(--top[0] > 0) pq.offer(top);
            } else {
                if(pq.isEmpty()) return "";
                int[] secTop = pq.poll();
                sb.append((char)secTop[1]);
                pos++;
                if(--secTop[0] > 0) pq.offer(secTop);
                pq.offer(top);
            }
        }
        return sb.toString();

    }
}
```
Time complexity = NlogN
Space complexity = N