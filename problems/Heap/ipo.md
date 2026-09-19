Prob: https://leetcode.com/problems/ipo/description/

Sol 1: Using Max heap
1. Take a array to sort the ipo by minimum capital required.
2. Sort the array in ascending order of capital required.
3. Create a max heap to store the maximum profit of the projects.
4. If initial capital is less than the minimum capital required, then return -1.
5. Add the maximum profit of the projects to the max heap.
```java
class Solution {
    public int findMaximizedCapital(int k, int w, int[] profits, int[] capital) {
        int idx = 0;
        int max = w;
        int[][] arr = new int[capital.length][2];
        PriorityQueue<Integer> pq = new PriorityQueue<>((elem1,elem2)->{
            return elem2 - elem1;
        });

        for(int i = 0; i < arr.length; i++) {
            arr[i][0] = capital[i];
            arr[i][1] = profits[i];
        }

        // Sort array
        Arrays.sort(arr,(elem1,elem2) ->{
            return elem1[0] - elem2[0];
        });

        while(k-- > 0) {
            while(idx < arr.length) {
                if(arr[idx][0] > max) break;
                pq.offer(arr[idx][1]);
                idx++;
            }
            if(pq.isEmpty()) break;
            max+= pq.poll();
        }
        return max;
    }
}
```
Time complexity = O(NlogK)
Space complexity = O(N)
