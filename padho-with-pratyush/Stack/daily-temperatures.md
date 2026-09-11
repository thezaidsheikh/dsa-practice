# Daily Temperatures

Prob: https://leetcode.com/problems/daily-temperatures/description/

Sol 1: Brute Force - Scan forward for each day
1. For each day i, scan every later day j.
2. Find the first j where temperatures[j] > temperatures[i].
3. Record j - i as the answer, else 0.

```java
class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        int n = temperatures.length;
        int[] res = new int[n];
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if (temperatures[j] > temperatures[i]) {
                    res[i] = j - i;
                    break;
                }
            }
        }
        return res;
    }
}
```
Time complexity - O(n^2),
Space complexity - O(1)

Sol 2: Better - Right-to-left using the answer array to jump
1. Initialize res[n - 1] = 0.
2. Traverse from right to left.
3. For each i, check day i + 1; if temperatures[i+1] > temperatures[i], the answer is 1.
4. Otherwise jump through the already-computed answers to reach a day warmer than i.
5. This skips many cold days using precomputed results.

```java
class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        int n = temperatures.length;
        int[] res = new int[n];
        for (int i = n - 2; i >= 0; i--) {
            int j = i + 1;
            while (temperatures[j] <= temperatures[i]) {
                if (res[j] == 0) {
                    j = n;
                    break;
                }
                j += res[j];
            }
            if (j < n) res[i] = j - i;
        }
        return res;
    }
}
```
Time complexity - O(n), amortized over all jumps
Space complexity - O(1)

Sol 3: Optimal - Monotonic stack
# Intuition
Moving from right to left, we want each index to know the nearest warmer day ahead. Maintaining a stack with temperatures in strictly decreasing order means the top of the stack is always the closest warmer day for the current index. Any index we pop is colder or equal and can never be the next warmer day for this element, so popping is safe forever.

1. Initialize the result array and push the last index onto the stack.
2. Iterate i from the second-last index down to 0.
3. While the stack is non-empty and the reference temperature <= current, pop (not warmer).
4. If the stack is empty, no warmer day exists to the right — answer is 0.
5. Otherwise the top is the nearest warmer day — answer is st.peek() - i.
6. Push the current index onto the stack.

```java
class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        int n = temperatures.length;
        int[] res = new int[n];
        Stack<Integer> st = new Stack<>();
        st.push(n - 1);

        for (int i = n - 2; i >= 0; i--) {
            int elem = temperatures[i];
            while (!st.isEmpty() && temperatures[st.peek()] <= elem) st.pop();
            if (st.isEmpty()) res[i] = 0;
            else res[i] = st.peek() - i;
            st.push(i);
        }
        return res;
    }
}
```
Time complexity - O(n), each index pushed and popped once
Space complexity - O(n), stack of indices