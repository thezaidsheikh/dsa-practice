Prob: https://www.naukri.com/code360/problems/ninja-and-sorted-arrays_1214628

Sol 1: Brute Force - Merge into a new array
1. Copy all m elements of arr1 into a temporary array.
2. Merge both arrays (temp and arr2) into a new result array of size m + n.
3. Copy the merged result back into arr1.
```java
import java.util.*;

public class Solution {
    public static int[] ninjaAndSortedArrays(int arr1[], int arr2[], int m, int n) {
        int[] temp = new int[m];
        for (int i = 0; i < m; i++) temp[i] = arr1[i];

        int i = 0, j = 0, k = 0;
        while (i < m && j < n) {
            if (temp[i] <= arr2[j]) arr1[k++] = temp[i++];
            else arr1[k++] = arr2[j++];
        }
        while (i < m) arr1[k++] = temp[i++];
        while (j < n) arr1[k++] = arr2[j++];

        return arr1;
    }
}
```
Time Complexity: O(m + n), where m is the number of valid elements in arr1 and n is arr2's length.
Space Complexity: O(m), for the temporary copy of arr1.

Sol 2: Better - Merge from the front with a new array
1. Create a result array of size m + n.
2. Use two pointers to merge arr1 and arr2 from the front.
3. Copy the merged result back into arr1.
```java
import java.util.*;

public class Solution {
    public static int[] ninjaAndSortedArrays(int arr1[], int arr2[], int m, int n) {
        int[] merged = new int[m + n];
        int i = 0, j = 0, k = 0;

        while (i < m && j < n) {
            if (arr1[i] <= arr2[j]) merged[k++] = arr1[i++];
            else merged[k++] = arr2[j++];
        }
        while (i < m) merged[k++] = arr1[i++];
        while (j < n) merged[k++] = arr2[j++];

        for (int p = 0; p < m + n; p++) arr1[p] = merged[p];

        return arr1;
    }
}
```
Time Complexity: O(m + n)
Space Complexity: O(m + n), for the merged array.

Sol 3: Optimal - Merge from the back
# Intuition
Both arr1 (extra space included) and arr2 are sorted. The largest elements sit at the end. By starting from the back of both arrays, we always know the next element to place is the larger of the two. This avoids any extra space since we fill arr1 from its last position backward.

1. Set three pointers: i = m-1 (last valid element of arr1), j = n-1 (last element of arr2), k = m+n-1 (end of the merged area).
2. Compare arr1[i] and arr2[j]; place the larger one at arr1[k] and move the respective pointer backward.
3. If arr1 is exhausted, copy the remaining elements of arr2.
```java
import java.util.*;

public class Solution {
    public static int[] ninjaAndSortedArrays(int arr1[], int arr2[], int m, int n) {
        int i = m - 1, j = n - 1, k = m + n - 1;

        while (j >= 0) {
            if (i >= 0 && arr1[i] > arr2[j])
                arr1[k--] = arr1[i--];
            else
                arr1[k--] = arr2[j--];
        }

        return arr1;
    }
}
```
Time Complexity: O(m + n)
Space Complexity: O(1), no extra space is used.

## Key Takeaways
- Merging from the back is the classic trick when arr1 has enough space to hold arr2's elements.
- Two-pointer from both ends avoids overwriting data we haven't read yet.
- Recognition cue: two sorted arrays with extra space at the end of arr1 -> merge from the right.
