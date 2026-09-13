# Ransom Note

Prob: https://leetcode.com/problems/ransom-note/description/

Sol 1: Brute Force - Find and consume each letter
1. For each character in ransomNote, scan magazine for a matching unused character.
2. Mark the found letter as used.
3. If any letter of ransomNote cannot be found, return false.

```java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        boolean[] used = new boolean[magazine.length()];
        for (char ch : ransomNote.toCharArray()) {
            boolean found = false;
            for (int j = 0; j < magazine.length(); j++) {
                if (!used[j] && magazine.charAt(j) == ch) {
                    used[j] = true;
                    found = true;
                    break;
                }
            }
            if (!found) return false;
        }
        return true;
    }
}
```
Time complexity - O(n * m),
Space complexity - O(m)

Sol 2: Better - Frequency hash map
1. Count the frequency of every letter in magazine in a HashMap.
2. For each letter in ransomNote, decrement its available count.
3. If any letter's count drops below 0, magazine cannot provide it — return false.

```java
import java.util.HashMap;

class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        HashMap<Character, Integer> count = new HashMap<>();
        for (char ch : magazine.toCharArray()) {
            count.put(ch, count.getOrDefault(ch, 0) + 1);
        }
        for (char ch : ransomNote.toCharArray()) {
            int left = count.getOrDefault(ch, 0);
            if (left == 0) return false;
            count.put(ch, left - 1);
        }
        return true;
    }
}
```
Time complexity - O(n + m),
Space complexity - O(1), at most 26 entries

Sol 3: Optimal - Frequency array of 26
# Intuition
Both strings use only lowercase English letters, so a fixed array of 26 acts like the map with O(1) access. First count what magazine has, then consume each letter for ransomNote — a negative count means magazine ran out of that letter.

1. Count the frequency of each letter in magazine using a size-26 array.
2. For each letter in ransomNote, decrement its count.
3. If any decrement makes the count negative, magazine lacks enough of that letter — return false.
4. If every letter is satisfied, return true.

```java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        int[] count = new int[26];
        for (char ch : magazine.toCharArray()) {
            count[ch - 'a']++;
        }
        for (char ch : ransomNote.toCharArray()) {
            count[ch - 'a']--;
            if (count[ch - 'a'] < 0) {
                return false;
            }
        }
        return true;
    }
}
```
Time complexity - O(n + m), two linear scans
Space complexity - O(1), fixed array of size 26