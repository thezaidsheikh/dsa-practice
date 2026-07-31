# padho-with-pratyush Problem Note Template

Standard structure for problem notes in this folder. This track holds fuller, intuition-heavy writeups with multiple solution tiers. The same format applies across its topic subfolders (`Binary-Search/`, `Heap/`, `Prefix-Sum/`, `Recursion/`, `Tree/`, `Graph/`).

## How to Use This Template
1. First line: `Prob:` followed by the problem URL.
2. Always write three solution tiers: `Sol 1: Brute Force`, `Sol 2: Better`, `Sol 3: Optimal`. Do not skip tiers.
3. Explain the intuition behind non-obvious approaches (`# Intuition`) before the code, and keep the numbered steps.
4. Use Java with `class Solution` (LeetCode style).
5. Close each solution with `Time complexity` and `Space complexity` lines; add a short explanation line when the complexity isn't obvious.
6. Place the note in the subfolder that matches its topic (Binary-Search, Heap, Prefix-Sum, Recursion, Tree, Graph).

## Template

````md
Prob: <problem-url>

Sol 1: Brute Force - <approach-name>
1. <step>
2. <step>

```java
class Solution {
    public <return-type> <method>(<args>) {
        // code
    }
}
```
Time complexity - O(...),
Space complexity - O(...)

Sol 2: Better - <approach-name>
1. <step>
2. <step>

```java
class Solution {
    // code
}
```
Time complexity - O(...),
Space complexity - O(...)

Sol 3: Optimal - <approach-name>
# Intuition
<why this approach works>

1. <step>
2. <step>

```java
class Solution {
    // code
}
```
Time complexity - O(...),
Space complexity - O(...)
```
````

## Filled Example (Subarray Sum Equals K)

```md
Prob: https://leetcode.com/problems/subarray-sum-equals-k/description/

Sol 1: Brute Force - Using all subarrays
1. Run a loop (i) that selects every possible starting index of the subarray.
2. Run a loop (j) that signifies the ending index of the subarray.
3. Calculate the sum of each subarray and check if it equals k.
4. If it does, increment the count.

```java
class Solution {
    public int subarraySum(int[] arr, int k) {
        int n = arr.length;
        int count = 0;
        for (int i = 0; i < n; i++) {
            for (int j = i; j < n; j++) {
                int sum = 0;
                for (int m = i; m <= j; m++) {
                    sum += arr[m];
                }
                if (sum == k) count++;
            }
        }
        return count;
    }
}
```
Time complexity - O(n^3),
Space complexity - O(1)

Sol 2: Better - Using two loops
1. Notice that the sum of the current subarray can be built incrementally.
2. As j moves forward, add arr[j] to the running sum instead of recomputing from scratch.

```java
class Solution {
    public int subarraySum(int[] arr, int k) {
        int n = arr.length;
        int count = 0;
        for (int i = 0; i < n; i++) {
            int sum = 0;
            for (int j = i; j < n; j++) {
                sum += arr[j];
                if (sum == k) count++;
            }
        }
        return count;
    }
}
```
Time complexity - O(n^2),
Space complexity - O(1)

Sol 3: Optimal - Using prefix sum
# Intuition
If the prefix sum up to index i is x, then the number of subarrays ending at i with sum k is exactly the number of times the prefix sum (x - k) has occurred before. Store prefix-sum frequencies in a map to count in one pass.

1. Maintain a running sum and a hash map of prefix-sum frequencies.
2. Initialize the map with prefix sum 0 occurring once.
3. For each element, check how many times (sum - k) has occurred and add it to the count.
4. Update the frequency of the current running sum.

```java
class Solution {
    public int subarraySum(int[] arr, int k) {
        HashMap<Integer, Integer> map = new HashMap<>();
        map.put(0, 1);
        int sum = 0, count = 0;
        for (int num : arr) {
            sum += num;
            count += map.getOrDefault(sum - k, 0);
            map.put(sum, map.getOrDefault(sum, 0) + 1);
        }
        return count;
    }
}
```
Time complexity - O(n),
Space complexity - O(n)
```
