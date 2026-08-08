# Dutch National Flag Algorithm

## Definition
The Dutch National Flag algorithm is a partitioning technique that rearranges an array containing three distinct values into three contiguous regions, each containing only one value. The algorithm completes in a single pass over the array without additional space, using three pointers to maintain invariants over distinct regions.

## When and Why to Use
- **When**: You need to partition an array with three distinct values (or colors) into three separate regions, or you need to rearrange an array based on three categories of elements.
- **Why**: Achieves O(n) time and O(1) extra space in a single pass, whereas naive sorting would take O(n log n) time. This is optimal for in-place partitioning problems with three groups.

## How It Works
### Core Mechanism
The algorithm maintains three pointers and three regions:
- **low**: boundary between region 0 and unclassified elements
- **mid**: current element being examined
- **high**: boundary between unclassified elements and region 2

The key invariant maintained throughout:
- Elements `[0, low)` contain value 0
- Elements `[low, mid)` contain value 1
- Elements `[mid, high]` contain unclassified elements
- Elements `(high, n-1]` contain value 2

At each step, we examine the element at `mid` and move exactly one pointer to maintain the invariant.

### Algorithm Steps
1. Initialize `low = 0`, `mid = 0`, `high = n - 1`
2. While `mid <= high`:
   - If `arr[mid] == 0`: swap `arr[mid]` and `arr[low]`, increment both `low` and `mid`
   - If `arr[mid] == 1`: increment `mid` (element is already in correct region)
   - If `arr[mid] == 2`: swap `arr[mid]` and `arr[high]`, decrement `high` (do NOT increment `mid`)
3. Array is now partitioned into three regions

### Simple Example
**Array**: `[2, 0, 2, 1, 1, 0]`

```
Initial: [2, 0, 2, 1, 1, 0]
         low=0, mid=0, high=5

Step 1: arr[0]=2, swap with arr[5]
        [0, 0, 2, 1, 1, 2]
        low=0, mid=0, high=4

Step 2: arr[0]=0, swap with arr[0], move both
        [0, 0, 2, 1, 1, 2]
        low=1, mid=1, high=4

Step 3: arr[1]=0, swap with arr[1], move both
        [0, 0, 2, 1, 1, 2]
        low=2, mid=2, high=4

Step 4: arr[2]=2, swap with arr[4]
        [0, 0, 1, 1, 2, 2]
        low=2, mid=2, high=3

Step 5: arr[2]=1, just move mid
        [0, 0, 1, 1, 2, 2]
        low=2, mid=3, high=3

Step 6: arr[3]=1, just move mid
        [0, 0, 1, 1, 2, 2]
        low=2, mid=4, high=3

mid > high, done!
Final: [0, 0, 1, 1, 2, 2]
```

### Code Implementation (Java)
```java
public void sortColors(int[] nums) {
    int low = 0, mid = 0, high = nums.length - 1;
    
    while (mid <= high) {
        if (nums[mid] == 0) {
            // Swap mid with low, both move forward
            swap(nums, mid, low);
            low++;
            mid++;
        } else if (nums[mid] == 1) {
            // Already in correct position, just move mid
            mid++;
        } else { // nums[mid] == 2
            // Swap mid with high, only high moves backward
            swap(nums, mid, high);
            high--;
        }
    }
}

private void swap(int[] nums, int i, int j) {
    int temp = nums[i];
    nums[i] = nums[j];
    nums[j] = temp;
}
```

## Important Insights and Definitions
- **Why not increment mid when swapping with high?**: When we swap `arr[mid]` with `arr[high]`, the element from the high region comes to mid's position and needs to be examined. The element we swapped out (which was 2) is now in the correct region, so we don't need to examine it again.
- **Single pass guarantee**: The algorithm completes in exactly one pass because each element is examined at most once. Once an element is classified, it moves to its final region and is not revisited.
- **Generalization to k values**: The algorithm extends to k distinct values using k pointers (complexity remains O(n) time, O(k) space for pointers).
- **In-place guarantee**: No extra array space is needed; all operations are swaps within the original array.

## Complexity Analysis
- **Time**: O(n) where n is the length of the array. Each element is visited at most once as the `mid` pointer moves from 0 to n-1.
- **Space**: O(1) auxiliary space (only three pointers, no additional data structures).

## Edge Cases to Watch For
- **Array with only one or two values**: Algorithm still works; unused regions will be empty. For example, array `[1, 1, 1]` remains unchanged since there are no 0s or 2s to place.
- **Array with one element**: `low=mid=0, high=0`, loop executes once and exits, correctly returning a single-element array.
- **Already sorted array**: Algorithm still makes swaps (redundant but harmless), and completes correctly.
- **All elements the same value**: No swaps occur; the array remains unchanged and correctly partitioned.
- **Empty array**: Loop condition `mid <= high` is false immediately (high = -1), so array remains unchanged.

## Differences from Other Patterns
- **vs. Quick Sort Partition**: Quick sort partitions around a single pivot (two regions) while Dutch National Flag partitions into three regions simultaneously. DNF is simpler and doesn't require choosing a pivot.
- **vs. Counting Sort**: Counting sort requires O(n + k) space for counting frequencies, whereas DNF uses O(1) space. DNF is superior when space is constrained.
- **vs. Two Pointers (opposite direction)**: Two pointers swap elements moving inward from both ends, while DNF uses three pointers with different movement rules. DNF handles three categories naturally.
- **vs. Bucket Sort**: Bucket sort distributes into k buckets (O(n + k) space), while DNF partitions in-place with only three pointers.

## Recognition Cues / Template Questions
- "Does the problem involve partitioning an array into regions based on three distinct values?"
- "Is the problem asking to sort colors or group 0s, 1s, and 2s?"
- "Do I need to partition in a single pass without extra space?"
- "Are there exactly three categories of elements that need to be separated?"
- "Can I use three pointers to define regions instead of sorting the entire array?"

## Type of Questions Solved
- **LeetCode 75: Sort Colors**: Classic problem where 0s, 1s, and 2s must be arranged in order.
- **Partitioning problems**: Separate elements based on three categories (e.g., negative, zero, positive numbers).
- **Array organization**: Group items by type where three types exist.
- **Flag/color variants**: Real-world problems where items are labeled with three states.

## Limitations / When Not to Use
- **More than three values**: For k > 3 distinct values, use generic k-way partitioning or quicksort.
- **Unknown distinct values at compile time**: If the count or values are dynamic, DNF's hardcoded logic (0, 1, 2) won't work; use a map-based approach.
- **Preserving relative order**: DNF does not preserve the original order of equal elements. Use stable sorting if relative order matters.
- **Linked lists**: DNF relies on random-access swaps; for linked lists, collect, partition, and reconstruct, or use a different approach.
- **Comparison-based need**: DNF works only with values that can be compared to fixed targets (0, 1, 2). For arbitrary comparators, use quicksort.

## Key Takeaways
- Dutch National Flag algorithm partitions an array into three regions in a single O(n) pass using O(1) space by maintaining three pointers and moving them according to deterministic rules based on the element at mid.
- The critical insight is that when swapping with the high pointer, we do not increment mid because the swapped-in element needs examination, but when swapping with low, both pointers advance because the element at low is guaranteed to be correctly placed.
- This is the optimal solution for three-way partitioning and should be recognized whenever a problem asks to group or sort three categories of elements in-place.
