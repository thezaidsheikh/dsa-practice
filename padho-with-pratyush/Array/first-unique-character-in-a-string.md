# First Unique Character In A String

Prob: https://leetcode.com/problems/first-unique-character-in-a-string/description/

Sol 1: Brute Force - Count occurrences on the fly
1. For each index i, scan the whole string counting how many times s.charAt(i) appears.
2. If the count is exactly 1, return i.
3. If no character has count 1, return -1.

```java
class Solution {
    public int firstUniqChar(String s) {
        int n = s.length();
        for (int i = 0; i < n; i++) {
            char ch = s.charAt(i);
            int count = 0;
            for (int j = 0; j < n; j++) {
                if (s.charAt(j) == ch) count++;
            }
            if (count == 1) return i;
        }
        return -1;
    }
}
```
Time complexity - O(n^2),
Space complexity - O(1)

Sol 2: Better - Frequency hash map
1. Count the frequency of every character in a HashMap.
2. Scan the string left to right again.
3. Return the first index whose character has frequency 1.
4. If none exists, return -1.

```java
import java.util.HashMap;

class Solution {
    public int firstUniqChar(String s) {
        HashMap<Character, Integer> freq = new HashMap<>();
        for (char ch : s.toCharArray()) {
            freq.put(ch, freq.getOrDefault(ch, 0) + 1);
        }
        for (int i = 0; i < s.length(); i++) {
            if (freq.get(s.charAt(i)) == 1) return i;
        }
        return -1;
    }
}
```
Time complexity - O(n),
Space complexity - O(n)

Sol 3: Optimal - Frequency array of 26
# Intuition
The string only contains lowercase English letters, so a fixed-size array of 26 replaces the map entirely. Each index maps a letter to its count with O(1) access, and the constant-size array means space stays O(1) regardless of input length.

1. Declare an int array of size 26 for the lowercase letters.
2. First pass: increment the count for each character using ch - 'a' as the index.
3. Second pass: scan the string again and return the first index whose count is exactly 1.
4. If no index qualifies, return -1.

```java
class Solution {
    public int firstUniqChar(String s) {
        int n = s.length();
        int[] arr = new int[26];

        for (int i = 0; i < n; i++) {
            arr[s.charAt(i) - 'a']++;
        }

        for (int i = 0; i < n; i++) {
            if (arr[s.charAt(i) - 'a'] == 1) {
                return i;
            }
        }

        return -1;
    }
}
```
Time complexity - O(n), two linear passes
Space complexity - O(1), fixed array of size 26