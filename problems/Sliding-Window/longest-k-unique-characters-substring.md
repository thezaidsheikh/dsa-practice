Prob: https://www.geeksforgeeks.org/problems/longest-k-unique-characters-substring0853/1

Sol 1: Brute Force - Checking all substrings
1. Run a loop (i) to pick the starting index of every substring.
2. Run a loop (j) from i onward, collecting characters into a set.
3. If the set size exceeds k, stop extending this substring.
4. If the set size is exactly k, record the length j - i + 1 and track the maximum.
5. Return -1 if no substring with exactly k distinct characters was found.
```java
class Solution {
    public int longestKSubstr(String s, int k) {
        int n = s.length();
        int max = Integer.MIN_VALUE;

        for (int i = 0; i < n; i++) {
            Set<Character> set = new HashSet<>();
            for (int j = i; j < n; j++) {
                set.add(s.charAt(j));
                if (set.size() > k) break;
                if (set.size() == k) {
                    max = Math.max(max, j - i + 1);
                }
            }
        }

        return max == Integer.MIN_VALUE ? -1 : max;
    }
}
```
Time Complexity: O(n^2), every start-end pair is examined.
Space Complexity: O(k), for the set of distinct characters.

Sol 2: Better - Using frequency array for all substrings
1. Same two-pointer generation of substrings, but count characters with an array instead of a set.
2. Maintain a distinct count as characters enter and leave the window instead of recomputing distinct characters.
3. This drops a hidden factor but is still quadratic overall.
```java
class Solution {
    public int longestKSubstr(String s, int k) {
        int n = s.length();
        int max = Integer.MIN_VALUE;

        for (int i = 0; i < n; i++) {
            int[] freq = new int[26];
            int distinct = 0;
            for (int j = i; j < n; j++) {
                int idx = s.charAt(j) - 'a';
                if (freq[idx] == 0) distinct++;
                freq[idx]++;
                if (distinct > k) break;
                if (distinct == k) {
                    max = Math.max(max, j - i + 1);
                }
            }
        }

        return max == Integer.MIN_VALUE ? -1 : max;
    }
}
```
Time Complexity: O(n^2), still checking all start-end pairs.
Space Complexity: O(1), fixed-size frequency array.

Sol 3: Optimal - Sliding window with HashMap
# Intuition
Maintain a window that always contains at most k distinct characters. Expand the right end to grow the window; if the window ever holds more than k distinct characters, shrink from the left until it is valid again. Whenever the window holds exactly k distinct characters, it is a candidate for the answer. The left pointer only moves forward, so every character enters and leaves the window at most once.

1. Expand the window by adding s[high] to a frequency map.
2. While the map has more than k distinct characters, remove characters from the left until the window is valid again.
3. When the map has exactly k distinct characters, update max with the current window length.
4. Return max, or -1 if it was never updated.
```java
class Solution {
    public int longestKSubstr(String s, int k) {
        int n = s.length();
        Map<Character, Integer> mp = new HashMap<>();
        int max = Integer.MIN_VALUE;
        int low = 0;

        for (int high = 0; high < n; high++) {
            char ch = s.charAt(high);
            mp.put(ch, mp.getOrDefault(ch, 0) + 1);

            while (mp.size() > k) {
                char lowCh = s.charAt(low);
                mp.put(lowCh, mp.get(lowCh) - 1);
                if (mp.getOrDefault(lowCh, 0) <= 0) mp.remove(lowCh);
                low++;
            }

            if (mp.size() == k) {
                int res = high - low + 1;
                max = Math.max(res, max);
            }
        }

        return max == Integer.MIN_VALUE ? -1 : max;
    }
}
```
Time Complexity: O(n), each character is added once and removed once.
Space Complexity: O(k), for the frequency map of at most k distinct characters.

## Key Takeaways
- Variable-size sliding window where the validity condition is on the number of distinct characters.
- The exact check `mp.size() == k` matters: some versions ask for "at most k", but this one requires exactly k distinct characters.
- Recognition cue: "longest substring with exactly k unique characters" -> sliding window + frequency map.
