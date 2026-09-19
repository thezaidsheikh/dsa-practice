Prob: https://leetcode.com/problems/majority-element-ii/description/

Sol 1: Brute Force - Using two loops
1. Initialize an empty list result.
2. For each element nums[i]:
3. Count how many times it appears in the array.
4. If count > n/3 and not already in result:
5. Add it to result.
6. Return result.
```java
import java.util.*;

class Solution {
    public List<Integer> majorityElement(int[] nums) {
        
        List<Integer> result = new ArrayList<>();
        int n = nums.length;
        
        for (int i = 0; i < n; i++) {
            
            // Skip if already added
            if (result.contains(nums[i])) {
                continue;
            }
            
            int count = 0;
            
            // Count frequency of nums[i]
            for (int j = 0; j < n; j++) {
                if (nums[j] == nums[i]) {
                    count++;
                }
            }
            
            // Check condition
            if (count > n / 3) {
                result.add(nums[i]);
            }
        }
        
        return result;
    }
}
```
Time Complexity = O(n^2)
Space Complexity = O(1) (ignoring output list as it smaller)

Sol 2: Better Approach - Using Map
1. Create an empty HashMap<Integer, Integer>.
2. Calculate limit = n / 3.
3. Traverse the array once:
4. Increase frequency in map.
5. If frequency becomes limit + 1, add element to result.
6. Return result list.
```java
class Solution {
    public List<Integer> majorityElement(int[] nums) {
        // Better Approach
        List<Integer> ls = new ArrayList<>();
        Map<Integer,Integer> mp = new HashMap<>();
        int n = nums.length / 3;
        for(int i = 0; i < nums.length; i++) {
            mp.put(nums[i], mp.getOrDefault(nums[i],0) + 1);

            if(mp.get(nums[i]) == n + 1) ls.add(nums[i]);
        }
        return ls;
    }
}
```
Time Complexity = O(N)
Space Complexity = O(N) (for HashMap)