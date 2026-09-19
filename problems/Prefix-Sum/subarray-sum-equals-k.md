Prob: https://leetcode.com/problems/subarray-sum-equals-k/description/

Sol 1: Brute Force - Using taking all the sub array and then find the sum
1. First, we will run a loop(say i) that will select every possible starting index of the subarray. The possible starting indices can vary from index 0 to index n-1(n = size of the array).
2. Inside the loop, we will run another loop(say j) that will signify the ending index of the subarray. For every subarray starting from the index i, the possible ending index can vary from index i to n-1(n = size of the array).
3. After that for each subarray starting from index i and ending at index j (i.e. arr[i….j]), we will run another loop to calculate the sum of all the elements(of that particular subarray).
4. After calculating the sum, we will check if the sum is equal to the given k. If it is, we will increase the value of the count.
```java
class Solution {
    // Function to find count of subarrays with sum equal to k
    public int subarraySum(int[] arr, int k) {
        // Size of the array
        int n = arr.length;

        // Initialize count of subarrays
        int count = 0;

        // Traverse all possible start indices
        for (int i = 0; i < n; i++) {
            // Traverse all possible end indices from start
            for (int j = i; j < n; j++) {
                // Initialize sum for current subarray
                int sum = 0;

                // Calculate sum of subarray from i to j
                for (int m = i; m <= j; m++) {
                    sum += arr[m];
                }

                // If sum equals k, increment count
                if (sum == k) {
                    count++;
                }
            }
        }

        // Return total count of subarrays
        return count;
    }
}
```
Time Complexity = O(N3), where N = size of the array.We are using three nested loops here. Though all are not running for exactly N times, the time complexity will be approximately O(N3).
Space Complexity = O(1) as we are not using any extra space.

Sol 2: Better approach - Using two loops
If we carefully observe, we can notice that to get the sum of the current subarray we just need to add the current element(i.e. arr[j]) to the sum of the previous subarray i.e. arr[i….j-1]. Assume previous subarray = arr[i……j-1]
current subarray = arr[i…..j]
Sum of arr[i….j] = (sum of arr[i….j-1]) + arr[j] This is how we can remove the third loop and while moving j pointer, we can calculate the sum.
```java
class Solution {
    // Function to find count of subarrays with sum equal to k
    public int subarraySum(int[] arr, int k) {
        // Size of the array
        int n = arr.length;

        // Initialize count of subarrays
        int count = 0;

        // Traverse all possible start indices
        for (int i = 0; i < n; i++) {
            // Initialize sum for current subarray
            int sum = 0;

            // Traverse all possible end indices from start
            for (int j = i; j < n; j++) {
                // Add current element to sum
                sum += arr[j];

                // If sum equals k, increment count
                if (sum == k) {
                    count++;
                }
            }
        }

        // Return total count of subarrays
        return count;
    }
}
```
Time Complexity: O(n²),We use two nested loops to check all subarrays, where n is the size of the array.
Space Complexity: O(1),Only a few extra variables are used, so constant extra space regardless of input size.

Sol 3: Optimal Approach - Using prefix sum
# Intuition
In this approach, we are going to use the concept of the prefix sum to solve this problem. Here, the prefix sum of a subarray ending at index i simply means the sum of all the elements of that subarray.
Assume, the prefix sum of a subarray ending at index i is x. In that subarray, we will search for another subarray ending at index i, whose sum equals k. Here, we need to observe that if there exists another subarray ending at index i with sum k, then the prefix sum of the rest of the subarray will be x-k. The below image will clarify the concept:
Now, for a subarray ending at index i with the prefix sum x, if we remove the part with the prefix sum x-k, we will be left with the part whose sum is equal to k. And that is what we want. Now, there may exist multiple subarrays with the prefix sum x-k. So, the number of subarrays with sum k that we can generate from the entire subarray ending at index i, is exactly equal to the number of subarrays with the prefix sum x-k, that we can remove from the entire subarray.
That is why, instead of searching the subarrays with sum k, we will keep the occurrence of the prefix sum of the subarrays using a map data structure.
In the map, we will store every prefix sum calculated, with its occurrence in a <key, value> pair. Now, at index i, we just need to check the map data structure to get the number of times that the subarrays with the prefix sum x-k occur. Then we will simply add that number to our answer.
```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int n = nums.length;
        int sum = 0;
        int count = 0;
        Map<Integer, Integer> freq = new HashMap<>();
        freq.put(0, 1);

        for (int i = 0; i < n; i++) {
            sum += nums[i];
            int need = sum - k;
            count += freq.getOrDefault(need, 0);

            // Record this sum occurrence
            freq.put(sum, freq.getOrDefault(sum, 0) + 1);
        }
        return count;
    }
}
```
Time Complexity: O(n), single pass; each prefix-sum update and `getOrDefault` lookup is O(1) on average.
Space Complexity: O(n), the hash map stores each distinct prefix sum (worst case: all prefix sums are unique).