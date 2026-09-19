# Remove All Adjacent Duplicates In String

Prob: https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/

Sol 1: Brute Force - Repeatedly scan and remove pairs
1. Keep scanning the string until no adjacent equal pair remains.
2. For each pass, if the current character equals the next one, drop both and restart the scan.
3. Repeat until a full pass finds no adjacent duplicate.

```java
class Solution {
    public String removeDuplicates(String s) {
        boolean changed = true;
        while (changed) {
            changed = false;
            StringBuilder sb = new StringBuilder();
            for (int i = 0; i < s.length(); i++) {
                if (i + 1 < s.length() && s.charAt(i) == s.charAt(i + 1)) {
                    i++;
                    changed = true;
                } else {
                    sb.append(s.charAt(i));
                }
            }
            s = sb.toString();
        }
        return s;
    }
}
```
Time complexity - O(n^2) in the worst case,
Space complexity - O(n)

Sol 2: Better - StringBuilder used as a stack
1. Iterate through each character.
2. Keep the result in a StringBuilder and always look at its last char.
3. If the current char matches the last char, delete the last char (simulates pop).
4. Otherwise append the current char (simulates push).

```java
class Solution {
    public String removeDuplicates(String s) {
        StringBuilder sb = new StringBuilder();
        for (char ch : s.toCharArray()) {
            if (sb.length() > 0 && sb.charAt(sb.length() - 1) == ch) {
                sb.deleteCharAt(sb.length() - 1);
            } else {
                sb.append(ch);
            }
        }
        return sb.toString();
    }
}
```
Time complexity - O(n),
Space complexity - O(n)

Sol 3: Optimal - Using a Stack
# Intuition
When the current character equals the top of the stack, this is an adjacent duplicate pair that cancels out — push builds the result, pop cancels it. What remains on the stack is exactly the string after all removals.

1. Iterate through each character of the string.
2. If the stack is empty, push the character.
3. Else compare the character with the top of the stack.
4. If they match, pop the top (the pair cancels).
5. Else push the character.
6. Pop everything into a StringBuilder and reverse it to restore order.

```java
class Solution {
    public String removeDuplicates(String s) {
        Stack<Character> st = new Stack<>();
        for (int i = 0; i < s.length(); i++) {
            char ch = s.charAt(i);
            if (st.isEmpty()) {
                st.push(ch);
                continue;
            }
            if (ch == st.peek()) st.pop();
            else st.push(ch);
        }
        StringBuilder res = new StringBuilder();
        while (!st.isEmpty()) {
            res.append(st.pop());
        }
        return res.reverse().toString();
    }
}
```
Time complexity - O(n), single pass
Space complexity - O(n), stack holds at most n characters