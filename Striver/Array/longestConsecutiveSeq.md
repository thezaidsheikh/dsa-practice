

Sol 1: Brute force - Using loop
1. Take a elem and loop through an array to check the next consecutive elem
2. If the next consecutive elem is found, then increment the count
3. If the next consecutive elem is not found, then reset the count
4. Return the max count

```java
import java.io.*;
import java.util.* ;

public class Solution {
    public static boolean linearSearch(int[] arr,int n, int preElem) {
        for(int i = 0; i < n; i++) {
            if(arr[i] == preElem) continue;
            else if(arr[i] == preElem + 1) return true;
        }
        return false;
    }
    public static int lengthOfLongestConsecutiveSequence(int[] arr, int N) {
        int longest = 1;
        // Brute force
        for(int i = 0; i < N; i++) {
            int count = 1;
            int elem = arr[i];
            while(linearSearch(arr,N,elem)) {
                count++;
                elem += 1;
            }
            if(count > longest) longest = count;
        }
        return longest;
    }
}
```
Time complexity - O(nlogn) + O(n^2) = O(n^2)
Space complexity - O(1)

Sol 2: Optimized - Using sorting
1. Sort the array
2. Take a prevElem and loop through an array to check the next consecutive elem
3. If the next consecutive elem is found, then increment the count
4. If the next consecutive elem is not found, then reset the count
5. Return the max count

```java
import java.io.*;
import java.util.* ;

public class Solution {
    public static int lengthOfLongestConsecutiveSequence(int[] arr, int N) {
        Arrays.sort(arr);

        int start = arr[0];
        int length = 1;
        int ans = Integer.MIN_VALUE;
        for(int i = 1; i < arr.length; i++) {
            int elem = arr[i];
            if(elem == start + 1) length++;
            else if(elem == start) {}
            else {
                ans = Math.max(ans,length);
                length = 1;
            }
            start = elem;
        }
        return ans = Math.max(ans,length);
    }
}
```
Time complexity - O(nlogn) + O(n) = O(nlogn)
Space complexity - O(1)

Sol 3: Optimal - Using HashSet
1. Store all the elements in the HashSet
2. Take a elem and check if its previous element is present in the HashSet or not.
3. If it's not present then check its consecutive elements (assuming it is a starting element).
4. If present then skip it.
5. Maintain the count and return the longest sequence

```java
import java.io.*;
import java.util.* ;

public class Solution {
    public static int lengthOfLongestConsecutiveSequence(int[] arr, int N) {
        int longest = 1;
        Set<Integer> set = new HashSet<>();
        for(int i = 0; i < arr.length; i++) set.add(arr[i]);

        for(int num: set) {
            if (!set.contains(num - 1)) {
                int currentNum = num;
                int currentStreak = 1;

                // 3. Count how long the sequence goes: num, num+1, num+2, ...
                while (set.contains(currentNum + 1)) {
                    currentNum++;
                    currentStreak++;
                }

                // 4. Update longest length
                longest = Math.max(longest, currentStreak);
            }
        }
        return longest;
    }
}
```
Time complexity - O(n) + O(n) (while loop is small compared to for loop) = O(n)
Space complexity - O(n)