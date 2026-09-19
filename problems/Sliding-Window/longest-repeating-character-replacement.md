Prob: https://leetcode.com/problems/longest-repeating-character-replacement/

Sol 1: Brute Force - Checking all substrings
1. Run a loop (i) to pick the starting index of every substring.
2. Run a loop (j) from i onward, counting character frequencies for the substring.
3. A substring is valid if we can make all its characters equal with at most k replacements, i.e. length - maxFrequency <= k.
4. Track the maximum length of a valid substring.
```java
class Solution {
    public int characterReplacement(String s, int k) {
        int n = s.length();
        int maxLength = 0;

        for (int i = 0; i < n; i++) {
            int[] freq = new int[26];
            for (int j = i; j < n; j++) {
                freq[s.charAt(j) - 'A']++;
                int maxFreq = 0;
                for (int f : freq) maxFreq = Math.max(maxFreq, f);

                if ((j - i + 1) - maxFreq <= k) {
                    maxLength = Math.max(maxLength, j - i + 1);
                }
            }
        }

        return maxLength;
    }
}
```
Time Complexity: O(n^3), scanning each substring and its frequencies.
Space Complexity: O(1), fixed-size frequency array.

Sol 2: Better - Tracking max frequency incrementally
1. Still check all substrings with two loops, but maintain the max frequency as characters are added instead of recomputing it.
2. For each substring, if length - maxFreq <= k, it is valid; update the answer.
```java
class Solution {
    public int characterReplacement(String s, int k) {
        int n = s.length();
        int maxLength = 0;

        for (int i = 0; i < n; i++) {
            int[] freq = new int[26];
            int maxFreq = 0;
            for (int j = i; j < n; j++) {
                freq[s.charAt(j) - 'A']++;
                maxFreq = Math.max(maxFreq, freq[s.charAt(j) - 'A']);

                if ((j - i + 1) - maxFreq <= k) {
                    maxLength = Math.max(maxLength, j - i + 1);
                }
            }
        }

        return maxLength;
    }
}
```
Time Complexity: O(n^2), all start-end pairs are checked.
Space Complexity: O(1), fixed-size frequency array.

Sol 3: Optimal - Sliding window
# Intuition
For any window, the number of replacements needed to make it all one character is window length minus the most frequent character's count: (j - i + 1) - maxFreq. If that cost exceeds k, the window is invalid and we shrink it. Because maxFreq only needs to be an upper bound on the true max (the window only grows towards the target answer), we never need to decrease it — shrinking the window when invalid is enough to keep the answer correct.

1. Expand the window by adding s[j] and update maxFreq.
2. While the window is invalid, i.e. (j - i + 1) - maxFreq > k, shrink from the left (decrement freq of s[i], move i forward).
3. The current window is now valid, so update maxLength.
4. Return maxLength.
```java
class Solution {
    public int characterReplacement(String s, int k) {
        int len = s.length();
        int[] freq = new int[26];
        int maxFreq = 0;
        int maxLength = 0;
        int i = 0;

        for (int j = 0; j < len; j++) {
            char ch = s.charAt(j);
            freq[ch - 'A']++;
            maxFreq = Math.max(maxFreq, freq[ch - 'A']);

            while ((j - i + 1) - maxFreq > k) {
                char lCh = s.charAt(i);
                freq[lCh - 'A']--;
                i++;
            }
            maxLength = Math.max(maxLength, j - i + 1);
        }

        return maxLength;
    }
}
```
Time Complexity: O(n), each character is added and removed at most once.
Space Complexity: O(1), fixed-size frequency array.

## Key Takeaways
- The key formula: replacements needed in a window = window length - maxFreq.
- maxFreq is never decremented, which keeps the solution correct and O(n); this works because we only shrink when invalid.
- Recognition cue: "longest substring where at most k changes make all characters equal" -> sliding window with a frequency array.
