Prob: https://leetcode.com/problems/majority-element/description

Sol 1: Brute force - Using 2 loops
1. First loop will track the element to check the count on.
2. Second loop will count the number of times that element occur.
3. If the total count of inner loop is greater then n/2 then it is a answer.
4. If not then return -1.

```java
class Solution {
    public int majorityElement(int[] nums) {
        int n = nums.length;

        for (int i = 0; i < n; i++) {
            int candidate = nums[i];
            int count = 0;

            for (int j = 0; j < n; j++) {
                if (nums[j] == candidate) {
                    count++;
                }
            }

            if (count > n / 2) {
                return candidate;
            }
        }

        // Problem guarantees majority exists, so we never actually reach here.
        return -1;
    }
}
```
Time complexity - O(n^2),
Space complexity - O(1)

Sol 2: Better Sol - Using map to store elements
1. Use a hash map to store the frequency of each number.
2. Iterate to map and check if number freq is greater then n/2 or not.

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int majorityElement(int[] nums) {
        Map<Integer, Integer> freq = new HashMap<>();
        int n = nums.length;

        for (int num : nums) {
            int count = freq.getOrDefault(num, 0) + 1;
            freq.put(num, count);

            if (count > n / 2) {
                return num;
            }
        }

        // Majority is guaranteed to exist
        return -1;
    }
}
```
Time complexity - O(n),
Space complexity - O(n)

Sol 3: Optimal Sol - Using Moore's algorithm
1. Think of it like votes canceling each other.
2. If two different elements meet → they cancel
3. The element with more than 50% votes can never be fully canceled
4. Find the majority element.
5. Verify the majority element. if satisfy the constraint then it is answer.
```java
class Solution {
    public int majorityElement(int[] nums) {
        // Moorey's algorithm
        int count = 1;
        int elem = nums[0];
        int n = nums.length;

        // Check elem who has not been cancelled
        for(int i = 1; i < n; i++) {
            if(count == 0) {
                count = 1;
                elem = nums[i];
            } else if(nums[i] == elem) count++;
            else count--;
        }

        // Verify the elem if it is a majority element
        int counter = 0;
        for(int i = 0; i < n; i++) {
            if(nums[i] == elem) counter++;
        }

        if(counter > n/2) return elem;
        return -1;
    }
}
```
Time complexity - O(n),
Space complexity - O(1)