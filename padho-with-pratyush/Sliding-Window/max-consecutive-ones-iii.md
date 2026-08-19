Prob: https://leetcode.com/problems/max-consecutive-ones-iii/description/

Sol 1: Brute Force - Checking all subarrays
1. Run a loop (i) to pick the starting index of every subarray.
2. Run a loop (j) from i onward, counting zeros in the current window.
3. If zeros exceed k, stop extending this subarray.
4. Otherwise update maxLength with j - i + 1.
```java
class Solution {
    public int longestOnes(int[] nums, int k) {
        int n = nums.length;
        int maxLength = 0;

        for (int i = 0; i < n; i++) {
            int zeros = 0;
            for (int j = i; j < n; j++) {
                if (nums[j] == 0) zeros++;
                if (zeros > k) break;
                maxLength = Math.max(maxLength, j - i + 1);
            }
        }

        return maxLength;
    }
}
```
Time Complexity: O(n^2), every start-end pair is examined.
Space Complexity: O(1)

Sol 2: Better - Sliding window counting zeros
1. Expand the window by moving the right pointer; increment zero count when a 0 is encountered.
2. While zero count exceeds k, shrink from the left; decrement zero count if a 0 leaves the window.
3. Update maxLength whenever the window is valid.
```java
class Solution {
    public int longestOnes(int[] nums, int k) {
        int n = nums.length;
        int left = 0, maxLength = 0;
        int zeros = 0;

        for (int right = 0; right < n; right++) {
            if (nums[right] == 0) zeros++;

            while (zeros > k) {
                if (nums[left] == 0) zeros--;
                left++;
            }

            maxLength = Math.max(maxLength, right - left + 1);
        }

        return maxLength;
    }
}
```
Time Complexity: O(n), each element enters and leaves the window at most once.
Space Complexity: O(1)

Sol 3: Optimal - Sliding window with frequency array
# Intuition
This is "longest subarray with at most k zeros" — or equivalently, we want the longest window whose length minus the most frequent element (the ones) is at most k. A frequency array of size 2 tracks the count of ones and zeros. We only shrink the window when it is no longer valid: (right - left + 1) - max_freq > k.

1. Maintain a frequency array for 0 and 1 in the current window, and track the highest frequency among them.
2. Expand the window to the right; update the frequency of the new element.
3. If the window has too many zeros, shrink from the left until valid again.
4. Update the answer with the current window size.
```java
class Solution {
    public int longestOnes(int[] nums, int k) {
        int n = nums.length;
        int max_num = 0;
        int max = Integer.MIN_VALUE;
        int left = 0;
        int[] freq = new int[2];

        for (int right = 0; right < n; right++) {
            int num = nums[right];
            freq[num]++;
            max_num = Math.max(max_num, freq[1]);

            while ((right - left + 1) - max_num > k) {
                freq[nums[left]]--;
                left++;
            }

            max = Math.max(max, right - left + 1);
        }

        return max;
    }
}
```
Time Complexity: O(n), each element is added and removed at most once.
Space Complexity: O(1), fixed-size frequency array.

## Key Takeaways
- Equivalent to "longest subarray with at most k zeros" — each 0 counts as one allowed flip.
- The pattern is identical to longest repeating character replacement (at most k changes), just with a binary input.
- Recognition cue: "at most k zeros in a subarray of 0s and 1s" -> sliding window.
