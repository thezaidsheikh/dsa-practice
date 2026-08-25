Prob: https://leetcode.com/problems/happy-number/description/

## Problem Summary
A happy number is a number defined by the process: replace it with the sum of the squares of its digits, and repeat. If this process ends at 1, the number is happy. If it loops endlessly in a cycle that does not include 1, it is not happy. Return whether `n` is a happy number.

Sol 1: Brute Force - HashSet of seen sums
1. Compute the next sum-of-squared-digits value repeatedly.
2. Store every sum seen in a HashSet. Before inserting, if the sum is already present, we've entered a cycle that does not reach 1 -> return false.
3. If a computed sum equals 1 at any point, the number is happy -> return true.

```java
class Solution {
    private int sum(int n) {
        int s = 0;
        while (n > 0) {
            int d = n % 10;
            n = n / 10;
            s = s + (d * d);
        }
        return s;
    }

    public boolean isHappy(int n) {
        HashSet<Integer> seen = new HashSet<>();
        while (n != 1 && seen.add(n)) {
            n = sum(n);
        }
        return n == 1;
    }
}
```
Time Complexity: O(log n) per step amortized; the sequence length is bounded, so effectively O(log n) work.
Space Complexity: O(log n), the set of distinct sums seen before 1 or a repeat.

Sol 2: Better - Unhappy cycle is fixed (check for 4)
# Intuition
It is a known mathematical fact that every unhappy number eventually falls into the SAME repeating cycle: 4 → 16 → 37 → 58 → 89 → 145 → 42 → 20 → 4. So instead of remembering every visited sum, we only need to watch for the landmark value 4 (or for 1). This drops the auxiliary space to O(1) without any fancy pointer math.

1. Compute successive sums.
2. Stop when the value becomes 1 (happy) or 4 (unhappy — trapped in the known cycle) and return accordingly.

```java
class Solution {
    private int sum(int n) {
        int s = 0;
        while (n > 0) {
            int d = n % 10;
            n = n / 10;
            s = s + (d * d);
        }
        return s;
    }

    public boolean isHappy(int n) {
        while (n != 1 && n != 4) {
            n = sum(n);
        }
        return n == 1;
    }
}
```
Time Complexity: O(log n) per step; bounded sequence length.
Space Complexity: O(1), only scalar variables.

Sol 3: Optimal - Floyd's Tortoise and Hare (slow/fast cycle detection)
# Intuition
The successive sums form a sequence that either converges to 1 (a "cycle" of one node) or enters a repeating cycle that never includes 1. This is exactly the cycle-detection shape of Linked List Cycle. Treat each sum as a "node" and `sum(x)` as its "next". Send two walkers at different speeds: slow takes one `sum` step, fast takes two. If there is a cycle, they must meet; if that meeting value is not 1, the cycle is the unhappy one -> return false. If slow ever reaches 1, the number is happy. No extra storage is needed.

1. Start slow and fast both at `n`.
2. While slow != 1: advance slow by one `sum` step and fast by two `sum` steps.
3. If slow == fast and slow != 1, a non-1 cycle was found -> return false.
4. If the loop exits because slow == 1, the number is happy -> return true.

```java
class Solution {
    int sum(int n) {
        int sum = 0;
        while (n > 0) {
            int d = n % 10;
            n = n / 10;
            sum = sum + (d * d);
        }
        return sum;
    }

    public boolean isHappy(int n) {
        int slow = n;
        int fast = n;

        while (slow != 1) {
            slow = sum(slow);
            fast = sum(fast);
            fast = sum(fast);

            if (slow == fast && slow != 1) {
                return false;
            }
        }
        return true;
    }
}
```
Time Complexity: O(log n) per step; the walk meets/terminates in a bounded number of iterations.
Space Complexity: O(1), only slow and fast scalars.

## Key Takeaways
- Happy-number checking is cycle detection on an abstract sequence — the same tortoise-and-hare idea as Linked List Cycle, just with `sum(x)` as the "next" link.
- The meeting guarantee inside a cycle comes from fast closing the gap by exactly 1 step per iteration, so it cannot leapfrog slow.
- Three ways to detect the cycle: remember all sums (HashSet, O(n) space), watch for the landmark 4 (O(1) space, needs the math fact), or use slow/fast pointers (O(1) space, general technique).
- Recognition cue: "does a process repeat / loop / cycle" with O(1) space -> think two-speed pointers before hash sets.
