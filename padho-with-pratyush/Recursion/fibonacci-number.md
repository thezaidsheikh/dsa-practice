Prob: https://leetcode.com/problems/fibonacci-number/

Sol 1: Optimized - Using recursion
1. For n = 1 fabonacci return 1.
2. For n = 0 fabonacci return 0.
3. Calculate for f(n-1) and f(n-2) and sum there answer.
```java
class Solution {
    public int fib(int n) {
        if(n == 1) return 1;
        if(n == 0) return 0;
        int num1 = fib(n-1);
        int num2 = fib(n-2);
        return num1 + num2;
    }
}
```
Time complexity = O(2^N)
Space complexity = O(N)

Sol 2: Optimal - Using loop and extra space
1. Take array to store the prev sum.
2. Initialize loop with 2.
```java
class Solution {
    public int fib(int n) {
        
        if(n <= 1)  return n;
        int[] dp = new int[n+1];
        dp[0] = 0;
        dp[1] = 1;

        for(int i = 2; i<=n; i++) {
            dp[i] = dp[i-1]+dp[i-2];
        }

        return dp[n];
    }
}
```
Time complexity = O(N)
Space complexity = O(N)