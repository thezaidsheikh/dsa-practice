Prob: https://leetcode.com/problems/longest-substring-without-repeating-characters/

Sol 1: Brute Force - Checking all substrings
1. Run a loop (i) to pick the starting index of every substring.
2. Run a loop (j) from i onward, adding characters to a set.
3. If a character repeats, stop extending this substring.
4. Track the maximum length of a substring with all distinct characters.
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length();
        int maxLength = 0;

        for (int i = 0; i < n; i++) {
            Set<Character> set = new HashSet<>();
            for (int j = i; j < n; j++) {
                if (set.contains(s.charAt(j))) break;
                set.add(s.charAt(j));
                maxLength = Math.max(maxLength, j - i + 1);
            }
        }

        return maxLength;
    }
}
```
Time Complexity: O(n^2), every start-end pair is examined.
Space Complexity: O(n), for the set.

Sol 2: Better - Sliding window with a frequency map
1. Expand the window by adding s[right].
2. While a character repeats (frequency > 1), shrink from the left until the window is valid again.
3. Update maxLength with the current window size.
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length();
        Map<Character, Integer> freq = new HashMap<>();
        int left = 0, maxLength = 0;

        for (int right = 0; right < n; right++) {
            char ch = s.charAt(right);
            freq.put(ch, freq.getOrDefault(ch, 0) + 1);

            while (freq.get(ch) > 1) {
                char lch = s.charAt(left);
                freq.put(lch, freq.get(lch) - 1);
                if (freq.get(lch) == 0) freq.remove(lch);
                left++;
            }

            maxLength = Math.max(maxLength, right - left + 1);
        }

        return maxLength;
    }
}
```
Time Complexity: O(n), each character enters and leaves the window at most once.
Space Complexity: O(1), at most 128 (or the alphabet size) distinct characters.

Sol 3: Optimal - Sliding window storing the last seen index
# Intuition
Instead of counting frequencies and shrinking repeatedly, store the last index (exclusive) where each character appeared. When a repeated character is met, jump `left` directly past its previous occurrence. Since each character's stored index is the latest position, this keeps the window valid in O(1) per character without a while loop.

1. Store for each character the index after its last occurrence.
2. For each character at `right`, set `left` to the max of itself and the stored index of the character (which is where its previous occurrence ends).
3. Update the stored index for this character to `right + 1`.
4. The window [left, right] has all distinct characters, so update maxLength.
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int[] charIndex = new int[128];

        int maxLength = 0, left = 0;

        for (int right = 0; right < s.length(); right++) {
            char ch = s.charAt(right);
            left = Math.max(left, charIndex[ch]);

            charIndex[ch] = right + 1;

            maxLength = Math.max(maxLength, right - left + 1);
        }

        return maxLength;
    }
}
```
Time Complexity: O(n), a single pass over the string.
Space Complexity: O(1), fixed-size array of 128.

## Key Takeaways
- Classic sliding window problem; the window is valid when all characters are distinct.
- The "last seen index" trick is the cleanest form: jump `left` instead of shrinking step by step.
- Recognition cue: "longest substring without repeating characters" -> sliding window with a visited-index array.
