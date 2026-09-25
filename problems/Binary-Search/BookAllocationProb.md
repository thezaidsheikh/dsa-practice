Prob: https://www.geeksforgeeks.org/problems/allocate-minimum-number-of-pages0937/1

Sol 1: Optimal Sol - Using Binary Search
1. Let's guess a max number of pages that can be allocated to a student.
2. The minimum possible guess is the max number of pages in a book.
3. The maximum possible guess is the sum of total pages.
4. Now we will check if the guess is possible or not.
5. If possible then we will try to find a smaller guess as we want to find the minimum number that is maximum number of pages that can be allocated to a student.
6. If not possible then we will try to find a greater guess.

```java
class Solution {
    public static boolean canAssign(int[] arr, int n, int k, long maxPages) {
        long student = 1;
        long pages = 0;
        
        for(int i = 0; i < n; i++) {
            long sum = arr[i] + pages;
            if(sum <= maxPages) {
                pages = sum;
            } else {
                student++;
                pages = arr[i];
                if(student > k) return false;
            }
        }
        return true;
    }
    public int findPages(int[] arr, int k) {
        int n = arr.length;
        if(n < k) return -1;
        long low = arr[0];
        long high = arr[0];
        long res = -1;
        
        // Find max element in array and sum of elements to guess the number.
        for(int i = 1; i < n; i++) {
            high += arr[i];
            if(arr[i] > low) low = arr[i];
        }
        
        while(low <= high) {
            long guess = (low + high) / 2;
            if(canAssign(arr, n, k, guess)) {
                res = guess;
                high = guess - 1;
            } else {
                low = guess + 1;
            }
        }
        
        return (int) res;
    }
}
```
Time complexity - O(n log m), each check is O(n) and binary search runs in O(log m) where m is the sum of pages
Space complexity - O(1)
