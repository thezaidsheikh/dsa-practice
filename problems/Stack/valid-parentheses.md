# Valid Parentheses

Prob: https://leetcode.com/problems/valid-parentheses/description/

Sol 1: Brute Force - Repeatedly remove innermost pairs
1. Repeatedly scan for the innermost matching pair `()`, `{}`, or `[]`.
2. Remove the pair and start over.
3. If a full pass finds no matching pair, stop.
4. The string is valid if we end up with an empty string.

```java
class Solution {
    public boolean isValid(String s) {
        boolean changed = true;
        while (changed) {
            changed = false;
            String t = s.replace("()", "").replace("{}", "").replace("[]", "");
            if (t.length() != s.length()) {
                changed = true;
                s = t;
            }
        }
        return s.isEmpty();
    }
}
```
Time complexity - O(n^2) in the worst case,
Space complexity - O(n)

Sol 2: Better - Stack matching top with current char
1. Push every opening bracket onto the stack.
2. When a closing bracket appears, the top of the stack must be its matching opening bracket.
3. If it matches, pop; otherwise the string is invalid.
4. At the end the stack must be empty.

```java
import java.util.HashMap;
import java.util.Map;
import java.util.Stack;

class Solution {
    public boolean isValid(String s) {
        Map<Character, Character> map = new HashMap<>();
        map.put(')', '(');
        map.put('}', '{');
        map.put(']', '[');

        Stack<Character> st = new Stack<>();
        for (char ch : s.toCharArray()) {
            if (map.containsKey(ch)) {
                if (st.isEmpty() || st.pop() != map.get(ch)) {
                    return false;
                }
            } else {
                st.push(ch);
            }
        }
        return st.isEmpty();
    }
}
```
Time complexity - O(n),
Space complexity - O(n)

Sol 3: Optimal - Stack with explicit matching checks
# Intuition
The most recent unclosed opening bracket must be the first to close — last-in, first-out. Keeping opening brackets on a stack means every closing bracket only needs to match the top of the stack; if it doesn't, the ordering is broken and the string can never be valid.

1. Push every opening bracket `(`, `{`, `[` onto the stack.
2. For a closing bracket, first check the stack is not empty.
3. Pop the top and verify it matches the expected opening bracket.
4. Any mismatch means invalid.
5. At the end the string is valid only if the stack is empty (no unclosed brackets remain).

```java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> st = new Stack<>();
        for (int i = 0; i < s.length(); i++) {
            char ch = s.charAt(i);
            if (ch == '(' || ch == '{' || ch == '[') {
                st.push(ch);
                continue;
            }
            if (st.isEmpty()) return false;
            char top = st.pop();
            if (ch == ')' && top != '(') return false;
            if (ch == '}' && top != '{') return false;
            if (ch == ']' && top != '[') return false;
        }
        return st.isEmpty();
    }
}
```
Time complexity - O(n), single pass
Space complexity - O(n), stack holds at most n characters