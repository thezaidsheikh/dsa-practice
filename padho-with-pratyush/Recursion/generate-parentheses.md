Prob: https://leetcode.com/problems/generate-parentheses/submissions/1926604211/

Sol 1: Using Recursion Backtracking

# Intuition:
# We want to generate all possible combinations of parentheses
# We can achieve this by using a recursive approach
# We can use backtracking, which is a technique to solve problems by trying all possible solutions
# We can use a recursive function to generate all possible combinations
# We start with an empty string and add opening parentheses and closing parentheses
# We have two conditions to break the recursion:
# 1. If the number of opening parentheses is equal to n, we have a valid combination
# 2. If the number of closing parentheses is greater than the number of opening parentheses, we have an invalid combination
# We can use a loop to add opening and closing parentheses to the string
# We can add opening parentheses when the number of opening parentheses is less than n
# We can add closing parentheses when the number of closing parentheses is less than the number of opening parentheses
# We can add opening parentheses to the string and recursively call the function with open + 1, close, n, str, ls
# We can add closing parentheses to the string and recursively call the function with open, close + 1, n, str, ls
# We can delete the last character from the string when we backtrack
# We can store all the valid combinations in a list

# Steps:
# Create a list to store all the valid combinations
# Create a recursive function to generate all possible combinations
# If the number of opening parentheses is equal to n, we have a valid combination, add the string to the list and return the list
# If the number of closing parentheses is greater than the number of opening parentheses, we have an invalid combination, return the list
# If the number of opening parentheses is less than n, add an opening parentheses to the string and recursively call the function with open + 1, close, n, str, ls
# If the number of closing parentheses is less than the number of opening parentheses, add a closing parentheses to the string and recursively call the function with open, close + 1, n, str, ls
# If none of the above conditions are met, return the list
```java
class Solution {
    public static List<String> rec(int open, int close, int n, StringBuilder str, List<String> ls) {
        if(open == n && close == n) {
            ls.add(str.toString());
            return ls;
        };

        if(open < n) {
            str.append("(");
            rec(open + 1, close, n, str, ls);
            str.deleteCharAt(str.length() - 1);
        }

        if(close < open) {
            str.append(")");
            rec(open, close + 1, n, str, ls);
            str.deleteCharAt(str.length() - 1);
        }
        return ls;
    }

    public List<String> generateParenthesis(int n) {
        StringBuilder str = new StringBuilder();
        List<String> ls = new ArrayList<>();
        return rec(0, 0, n, str, ls);
    }
}
```
Time complexity - O(4N/Sqrt(N))
The time complexity is exponential because for each opening parenthesis, we have two options: either to add a closing parenthesis or to not add a closing parenthesis. Also, the recursion tree has a depth of n, and for each level there are 2^n nodes.

Space complexity - O(n * 2^n)
The space complexity is also exponential because we store all the valid combinations in a list, and each combination has a length of n. Also, the recursion tree has a depth of n, and for each level there are 2^n nodes.