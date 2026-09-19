Prob: https://leetcode.com/problems/top-k-frequent-words/description/

Sol 1: Using heap
1. Insert element and its freq into hashmap
2. Create a min heap of size k as we have to find k elements
3. Insert k elements and its freq into heap.
4. After inserting k elements check the top element if it is less then current freq then it is obvious that it will be less than other freq on heap.
5. If it is bigger than remove the top element and insert this new element and its freq.
6. Ths final answer will be the remaining k elements in heap
```java
class Solution {
    public List<String> topKFrequent(String[] words, int k) {
        // Hashmap to store the words frequency
        Map<String,Integer> mp = new LinkedHashMap<>();

        // Min priority queue to store the most freq words
        PriorityQueue<String> pq = new PriorityQueue<>((a,b)->{
            int fa = mp.get(a);
            int fb = mp.get(b);
            if(fa != fb) {
                return fa - fb; // smaller frequency = smaller in heap
            }

            // for same freq: lexicographically *reverse*
            return b.compareTo(a); // so that larger word is smaller in heap
        });

        for(int i = 0; i < words.length; i++) {
            mp.put(words[i], mp.getOrDefault(words[i],0) + 1);
        }

        // 3. Keep only top k in heap
        for (String word : mp.keySet()) {
            pq.offer(word);
            if (pq.size() > k) {
                pq.poll(); // remove smallest (worst candidate)
            }
        }

        // 4. Extract from heap into result (heap gives smallest first,
        // but we need highest freq first, so reverse at the end)
        List<String> res = new LinkedList<>();
        while (!pq.isEmpty()) {
            res.addFirst(pq.poll());
        }
        return res;
    }
}
```