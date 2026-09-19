Prob: https://leetcode.com/problems/combination-sum/

Sol 1: Using recursion
1. This problem is about exploring all possible combinations that can make the target.
2. At every index, we have two choices: either take the current number or skip it.
3. If we skip the current number, we move to the next index.
4. If we take the current number, we stay on the same index because we can use the same number unlimited times.
5. We keep track of the current sum to know how close we are to the target.
6. If the current sum becomes equal to the target, we store the current list as one valid answer.
7. If the current sum exceeds the target, we stop exploring that path because it can never become valid.
8. If the index reaches the end of the array, we stop that branch.
9. We use backtracking by adding a number, exploring further, and then removing that number to try other possibilities.
10. Removing the last added element keeps the list clean for the next recursive branch.
11. We always add a copy of the current list to the result because the original list keeps changing during recursion.
12. This solution works like building a decision tree where each branch represents a choice of including or excluding a number.

## Step-by-Step Approach (Backtracking – Include/Exclude Pattern)
1. Start recursion with index = 0, sum = 0, and an empty list to store the current combination.
2. If index becomes equal to the length of the array, check whether sum equals target and if yes add a copy of the list to the result, otherwise return.
3. Call recursion by moving to index + 1 to represent skipping the current element.
4. Check if adding the current element keeps the sum less than or equal to the target.
5. If it is valid, add the current element to the list.
6. Increase the sum by the value of the current element.
7. Call recursion again with the same index because the element can be used unlimited times.
8. After the recursive call finishes, remove the last element from the list.
9. Decrease the sum to restore the previous state.
10. Continue this process until all recursion branches are explored.
11. Return the result list containing all valid combinations.

# Solution
```java
class Solution {
    public static void rec(int[] cand, int tar, List<Integer> ls, List<List<Integer>> res, int idx, int sum) {
        if(idx >= cand.length) {
            if(sum == tar) res.add(new ArrayList<>(ls));
            return;
        }

        rec(cand, tar, ls, res, idx+1, sum);

        if(cand[idx] + sum <= tar) {
            ls.add(cand[idx]);
            sum += cand[idx];
            rec(cand, tar, ls, res, idx, sum);
            ls.remove(ls.size() - 1);
            sum -= cand[idx];
        }
        return;
    }
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<Integer> ls = new ArrayList<>();
        List<List<Integer>> res = new ArrayList<>();
        int sum = 0;
        int idx = 0;
        rec(candidates, target, ls, res, idx, sum);
        return res;
    }
}
```
# Time and Space Complexity

## Time Complexity

Worst Case: O(2^T) approximately exponential

More precise form: O(N^(T/min)) ~ O(N^T)

Where:

* N = number of candidates
* T = target value
* min = smallest number in candidates

Reason:
We are exploring a recursion tree where each number can be chosen multiple times, and the depth can go up to T/min.

---

## Space Complexity

Auxiliary Space (Recursion Stack): O(T/min) ~ O(T)

Reason:
Maximum depth of recursion occurs when we keep choosing the smallest element.

Result Storage Space:
Depends on number of valid combinations (given constraint says less than 150).

Overall Space Complexity:
O(T/min) + space for storing results.
