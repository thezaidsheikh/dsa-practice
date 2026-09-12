# Remove All Adjacent Duplicates in String II

Prob: https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string-ii/description/

Sol 1: Brute Force - Repeatedly remove groups of k identical chars
1. Keep scanning the string until no group of k consecutive equal chars remains.
2. For each pass, find the first k consecutive equal characters and delete them.
3. Restart the scan after every deletion.
4. Repeat until a full pass finds nothing to delete.

```java
class Solution {
    public String removeDuplicates(String s, int k) {
        boolean changed = true;
        while (changed) {
            changed = false;
            StringBuilder sb = new StringBuilder();
            int i = 0;
            while (i < s.length()) {
                int j = i;
                while (j < s.length() && s.charAt(j) == s.charAt(i)) j++;
                int count = j - i;
                if (count >= k) {
                    changed = true;
                    i = j;
                } else {
                    while (count-- > 0) sb.append(s.charAt(i));
                    i = j;
                }
            }
            s = sb.toString();
        }
        return s;
    }
}
```
Time complexity - O(n^2 / k) in the worst case,
Space complexity - O(n)

Sol 2: Better - StringBuilder as a stack of [char, count]
1. Simulate a stack with a StringBuilder while tracking the run length of the current character.
2. Compare each character with the last appended character.
3. If it continues the run and reaches k, delete the last k - 1 characters.
4. Otherwise append and update the count.

```java
class Solution {
    public String removeDuplicates(String s, int k) {
        int[] count = new int[s.length()];
        StringBuilder sb = new StringBuilder();
        for (char ch : s.toCharArray()) {
            int idx = sb.length();
            if (idx > 0 && sb.charAt(idx - 1) == ch) {
                count[idx] = count[idx - 1] + 1;
                sb.append(ch);
                if (count[idx] == k) {
                    sb.setLength(idx - k + 1);
                }
            } else {
                count[idx] = 1;
                sb.append(ch);
            }
        }
        return sb.toString();
    }
}
```
Time complexity - O(n),
Space complexity - O(n)

Sol 3: Optimal - Stack of [char, count]
# Intuition
A plain stack can't handle the k-rule because we need to know how many consecutive same characters are on top. Storing each character together with its current run length lets us cancel an entire run when it reaches k — push either starts a new run or extends the top run, and a run can only be removed from the very top (LIFO order).

1. Iterate through each character.
2. If the stack is empty, push [ch, 1].
3. If the top character differs, push [ch, 1].
4. If the top matches and its count < k - 1, pop and push [ch, count + 1].
5. If the top matches and its count == k - 1, just pop — the run is fully removed.
6. Rebuild the string from the stack, repeating each char by its count, then reverse.

```java
class Solution {
    public String removeDuplicates(String s, int k) {
        int n = s.length();
        Stack<int[]> st = new Stack<>();

        for (int i = 0; i < n; i++) {
            char elem = s.charAt(i);

            if (st.isEmpty()) {
                st.push(new int[]{elem, 1});
                continue;
            }

            int[] top = st.peek();
            if (top[0] != elem) {
                st.push(new int[]{elem, 1});
                continue;
            }

            if (top[1] != k - 1) {
                st.pop();
                st.push(new int[]{elem, top[1] + 1});
                continue;
            }

            st.pop();
        }

        StringBuilder str = new StringBuilder();
        while (!st.isEmpty()) {
            int[] top = st.pop();
            while (top[1] > 0) {
                str.append((char) top[0]);
                top[1]--;
            }
        }
        return str.reverse().toString();
    }
}
```
Time complexity - O(n), each character pushed and popped once
Space complexity - O(n), stack holds at most n entries