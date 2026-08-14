Prob: https://leetcode.com/problems/fruit-into-baskets/description/

Sol 1: Brute Force - Checking all subarrays
1. Run a loop (i) to pick the starting index of every subarray.
2. Run a loop (j) from i onward, counting how many distinct fruit types appear.
3. If the distinct count exceeds 2, stop extending this subarray.
4. Otherwise update maxLen with j - i + 1.
```java
class Solution {
    public int totalFruit(int[] fruits) {
        int n = fruits.length;
        int maxLen = 0;

        for (int i = 0; i < n; i++) {
            Set<Integer> set = new HashSet<>();
            for (int j = i; j < n; j++) {
                set.add(fruits[j]);
                if (set.size() > 2) break;
                maxLen = Math.max(maxLen, j - i + 1);
            }
        }

        return maxLen;
    }
}
```
Time Complexity: O(n^2), every start-end pair is examined.
Space Complexity: O(1), the set never holds more than 3 types.

Sol 2: Better - Sliding window with HashMap
# Intuition
This is "longest subarray with at most 2 distinct values". Keep a window with at most 2 fruit types by expanding the right end and shrinking from the left whenever a third type appears. Every time the window is valid, its length is a candidate answer.

1. Expand the window by adding fruits[high] to a frequency map.
2. While the map has more than 2 distinct types, remove fruits from the left until the window is valid again.
3. Update max with the current window length.
4. Return max.
```java
class Solution {
    public int totalFruit(int[] fruits) {
        int n = fruits.length;
        if (n <= 2) {
            return n;
        }

        int start = 0;
        int high = 0;
        Map<Integer, Integer> mp = new LinkedHashMap<>();
        int max = Integer.MIN_VALUE;

        for (high = 0; high < n; high++) {
            mp.put(fruits[high], mp.getOrDefault(fruits[high], 0) + 1);
            while (mp.size() > 2) {
                mp.put(fruits[start], mp.get(fruits[start]) - 1);
                if (mp.get(fruits[start]) == 0) {
                    mp.remove(fruits[start]);
                }
                start++;
            }

            int sum = high - start + 1;
            max = Math.max(max, sum);
        }

        return max;
    }
}
```
Time Complexity: O(n), each element is added once and removed once.
Space Complexity: O(1), the map holds at most 2 distinct types.

Sol 3: Optimal - Sliding window tracking the last two fruit types
# Intuition
We only ever need to remember the two fruit types in the current valid window, not full frequencies. When a new fruit type appears that is neither of the two, start a fresh window that begins at the last occurrence of the previous fruit type. This drops the map entirely.

1. Keep track of `first` and `second`, the two fruit types in the current window.
2. Walk `right` through the array.
3. When a fruit matches one of the two types, just extend the window.
4. When a new third type appears, restart the window: start it at the last contiguous run of the previous fruit type, then set `first` and `second` to the last fruit type and the new type.
5. Update maxLen with the current window size.
```java
class Solution {
    public int totalFruit(int[] fruits) {
        int left = 0;
        int maxLen = 0;
        int first = fruits[0];
        int second = -1;

        for (int right = 0; right < fruits.length; right++) {
            if (fruits[right] != first && second == -1)
                second = fruits[right];

            if (fruits[right] != first && fruits[right] != second) {
                int lastfruit = fruits[right - 1];
                left = right - 1;

                while (left > 0 && fruits[left - 1] == lastfruit)
                    left--;

                first = fruits[left];
                second = fruits[right];
            }

            maxLen = Math.max(maxLen, right - left + 1);
        }

        return maxLen;
    }
}
```
Time Complexity: O(n), each element is visited at most a constant number of times.
Space Complexity: O(1), no extra data structure is used.

## Key Takeaways
- Fruit Into Baskets is exactly "longest subarray with at most 2 distinct values".
- The HashMap sliding window is the safe, general pattern; the two-variable version is a space-optimized variant that works because the window only ever contains 2 types.
- Recognition cue: "at most k distinct types / baskets" -> sliding window with a frequency map.
