Prob: https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/

Sol 1: Using Recursion
# Steps:
# 1. Initialize a map to store the mapping of each digit to its corresponding letters
# 2. Initialize a recursive function to generate all possible combinations
# 3. If the index is equal to the length of the digits, we have a valid combination, add the string to the list and return the list
# 4. Get the letters for the current digit
# 5. Add each letter to the string and recursively call the function with idx + 1, n, str, ls
# 6. Delete the last character from the string when we backtrack
# 7. Return the list of valid combinations
```java
class Solution {
    private static final Map<Character, String> mp = new HashMap<>();
    static {
        mp.put('2', "abc");
        mp.put('3', "def");
        mp.put('4', "ghi");
        mp.put('5', "jkl");
        mp.put('6', "mno");
        mp.put('7', "pqrs");
        mp.put('8', "tuv");
        mp.put('9', "wxyz");
    }
    public static List<String> rec(String digits, StringBuilder str, int idx,int n, List<String> ls) {
        if(idx == n) {
            ls.add(str.toString());
            return ls;
        }

        String choice = mp.get(digits.charAt(idx));

        for(int i = 0; i < choice.length(); i++) {
            str.append(choice.charAt(i));
            rec(digits, str, idx+1, n, ls);
            str.deleteCharAt(str.length() -1);
        }

        return ls;
    }
    public List<String> letterCombinations(String digits) {
        int n = digits.length();
        StringBuilder str = new StringBuilder("");
        int idx = 0;
        List<String> ls = new ArrayList<>();
        return rec(digits, str, idx , n, ls);

    }
}
```
Time complexity - O(4^N)
// The time complexity is exponential because for each digit, we have 4 options: either to add 'a', 'b', 'c', or 'd'. Also, the recursion tree has a depth of n, and for each level there are 4 nodes.
Space complexity - O(N * 4^N) ~ O(N)
// The space complexity is also exponential because we store all the valid combinations in a list, and each combination has a length of n. Also, the recursion tree has a depth of n, and for each level there are 4 nodes.