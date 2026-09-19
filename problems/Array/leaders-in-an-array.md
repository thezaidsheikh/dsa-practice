Prob: https://www.naukri.com/code360/problems/leaders-in-an-array_873144

Sol 1: Brute force - Using 2 for loops
1. Outer loop will be for element on which we will check the greater than operation.
2. Inner loop iterate to all the elements to the right to check the greater elem.
3. If any greater elem found then the current number is not a leader.
4. Else we will store the elem in our leader array.
```java
class Solution {
    public List<Integer> leaders(int[] nums) {
        List<Integer> leaders = new ArrayList<>();

        for (int i = nums.length - 1; i >= 0; i--) {
            boolean isLeader = true;
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[j] > nums[i]) {
                    isLeader = false;
                    break;
                }
            }
            if (isLeader) {
                leaders.add(nums[i]);
            }
        }

        Collections.reverse(leaders);
        return leaders;
    }
}
```

Sol 2: Optimal - Using two pointers
1. As the elem which is greater to all the elem in the right will be leader.
2. Let's first store the last elem in the leader.
3. Take 2 pointer i and j starting from n-1 and n-2
4. Pointer i will be on the greatest elem found from the right.
5. Pointer j will interate to the elem to look for next elem which is greater than the ith element and store it.
6. If found then i will take place of j.
7. Reverse the array so that we have leaders according to our input.
```java
class Solution {
    public List<Integer> leaders(int[] nums) {
        List<Integer> leaders = new ArrayList<>();
        int n = nums.length;
        int i = n - 1, j = n - 2;

        while (i >= 0) {
            if (nums[i] > nums[j]) {
                leaders.add(nums[i]);
                j = i;
            }
            i--;
        }

        Collections.reverse(leaders);
        return leaders;
    }
}
```