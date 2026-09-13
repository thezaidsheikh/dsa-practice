# Maximum Number of Balloons

Prob: https://leetcode.com/problems/maximum-number-of-balloons/

Sol 1: Brute Force - Build balloons one by one
1. Loop while the text still contains all letters needed for one "balloon".
2. Each pass consumes one b, one a, two l's, two o's, and one n.
3. Count how many full balloons were formed.

```java
class Solution {
    public int maxNumberOfBalloons(String text) {
        String balloon = "balloon";
        int count = 0;
        StringBuilder sb = new StringBuilder(text);
        boolean formed = true;
        while (formed) {
            for (char ch : balloon.toCharArray()) {
                int idx = sb.indexOf(String.valueOf(ch));
                if (idx == -1) {
                    formed = false;
                    break;
                }
                sb.deleteCharAt(idx);
            }
            if (formed) count++;
        }
        return count;
    }
}
```
Time complexity - O(n * k) where k is the number of balloons,
Space complexity - O(n)

Sol 2: Better - Frequency hash map
1. Count the frequency of every character in text using a HashMap.
2. For "balloon", count b, a, l, o, n as 1, 1, 2, 2, 1.
3. For l and o divide the available count by 2.
4. The minimum of these effective counts is the answer.

```java
import java.util.HashMap;

class Solution {
    public int maxNumberOfBalloons(String text) {
        HashMap<Character, Integer> freq = new HashMap<>();
        for (char ch : text.toCharArray()) {
            freq.put(ch, freq.getOrDefault(ch, 0) + 1);
        }
        int min = Integer.MAX_VALUE;
        for (char ch : "balloon".toCharArray()) {
            if (!freq.containsKey(ch)) return 0;
            int count = freq.get(ch);
            if (ch == 'l' || ch == 'o') count /= 2;
            min = Math.min(min, count);
        }
        return min;
    }
}
```
Time complexity - O(n),
Space complexity - O(1), at most 26 entries

Sol 3: Optimal - Frequency array of 26
# Intuition
"balloon" needs b:1, a:1, l:2, o:2, n:1. A size-26 array gives O(1) access to each letter's count. Because l and o appear twice per word, their count is halved; the smallest effective count across the five letters caps how many full words text can form.

1. If text is shorter than "balloon", return 0 immediately.
2. Count all character frequencies in a size-26 array.
3. Iterate over the letters of "balloon".
4. If any letter is missing (count 0), return 0.
5. For l and o, take count / 2 since the word uses two of each.
6. Track the minimum across all letters — that is the answer.

```java
class Solution {
    public int maxNumberOfBalloons(String text) {
        String balloon = "balloon";
        if (text.length() < balloon.length()) return 0;

        int min = Integer.MAX_VALUE;
        int[] arr = new int[26];
        for (int i = 0; i < text.length(); i++) {
            arr[text.charAt(i) - 'a']++;
        }

        for (int i = 0; i < balloon.length(); i++) {
            char ch = balloon.charAt(i);
            if (arr[ch - 'a'] == 0) return 0;
            if (ch == 'l' || ch == 'o') {
                min = Math.min(min, arr[ch - 'a'] / 2);
            } else {
                min = Math.min(min, arr[ch - 'a']);
            }
        }
        return min;
    }
}
```
Time complexity - O(n), two linear scans
Space complexity - O(1), fixed array of size 26