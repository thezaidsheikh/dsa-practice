Prob: https://www.geeksforgeeks.org/problems/allocate-minimum-number-of-pages0937/1

Sol 1: Optimal Sol - Using Merge Sort
1. Let's guess a max number of pages that can be allocated to a student.
2. Also the minimum number of pages that can be allocated to a student is the max number of pages in a book.
3. And the maximum number of pages that can be allocated to a student is the sum of total pages.
3. Now we will check if the guess is possible or not.
4. If possible then we will try to find a smaller guess as we want to find the minimum number that is maximum number of pages that can be allocated to a student.
5. If not possible then we will try to find a greater guess.

```java
class Solution {
    public static boolean canAssign(int[] arr, int n, int k, int maxPages) {
        int student = 1;
        int pages = 0;
        
        for(int i = 0; i < n; i++) {
            int sum = arr[i] + pages;
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
        int low = arr[0];
        int high = arr[0];
        int res = -1;
        
        // Find max element in array and sum of elements to guess the number.
        for(int i = 1; i < n; i++) {
            high += arr[i];
            if(arr[i] > low) low = arr[i];
        }
        
        while(low <= high) {
            int guess = (low + high) / 2;
            if(canAssign(arr, n, k, guess)) {
                res = guess;
                high = guess - 1;
            } else {
                low = guess + 1;
            }
        }
        
        return res;
    }
}
```
Time complexity - O(nlogm) + O(nlogm)
Space complexity - O(1)
