# Sum of Two Arrays

Prob: https://www.naukri.com/code360/problems/sum-of-two-arrays_893186

Sol 1: Brute Force - Converting arrays to numbers
1. Convert array a into a single number by repeated (num * 10 + digit).
2. Do the same for array b.
3. Add the two numbers and convert the result back into digits.
4. This is simple but breaks on large arrays — the number overflows even long.

```java
public class Solution {
    public static int[] findArraySum(int[] a, int n, int[] b, int m) {
        long num1 = 0, num2 = 0;
        for (int i = 0; i < n; i++) num1 = num1 * 10 + a[i];
        for (int i = 0; i < m; i++) num2 = num2 * 10 + b[i];
        long sum = num1 + num2;
        String s = String.valueOf(sum);
        int[] res = new int[s.length()];
        for (int i = 0; i < s.length(); i++) res[i] = s.charAt(i) - '0';
        return res;
    }
}
```
Time complexity - O(n + m),
Space complexity - O(1) (fails with overflow for large inputs)

Sol 2: Better - Building the result backwards with a list
1. Start from the last digit of both arrays and add with carry.
2. Push each resulting digit into a list as we go.
3. Reverse the list at the end since digits were produced right-to-left.
4. No overflow, but the list adds extra space.

```java
import java.util.*;

public class Solution {
    public static int[] findArraySum(int[] a, int n, int[] b, int m) {
        List<Integer> list = new ArrayList<>();
        int i = n - 1, j = m - 1, carry = 0;
        while (i >= 0 || j >= 0 || carry != 0) {
            int sum = carry;
            if (i >= 0) sum += a[i--];
            if (j >= 0) sum += b[j--];
            list.add(sum % 10);
            carry = sum / 10;
        }
        Collections.reverse(list);
        int[] res = new int[list.size()];
        for (int k = 0; k < res.length; k++) res[k] = list.get(k);
        return res;
    }
}
```
Time complexity - O(n + m),
Space complexity - O(n + m) (list stores the result digits)

Sol 3: Optimal - Writing directly into a fixed-size array
# Intuition
The sum of two arrays of size n and m can have at most max(n, m) + 1 digits (one extra for a final carry). Instead of building a list and reversing, we can pre-allocate an array of that exact size, fill it from the right, and strip the leading zero only if the extra slot was never used. Every digit is written exactly once with no reversal step.

1. Allocate a result array of size max(n, m) + 1.
2. Place pointers i and j at the last index of each array, and k at the last index of the result.
3. Loop while either pointer is in range or a carry remains.
4. Add the digits from a, b, and the carry, then write sum % 10 into the result and keep sum / 10 as the new carry.
5. After the loop, strip the leading zero if the extra slot wasn't needed.
6. Return the result.

```java
import java.util.* ;
import java.io.*;

public class Solution {
	public static int[] findArraySum(int[] a, int n, int[] b, int m) {
		int size = Math.max(n, m) + 1;   // result is at most max(n,m)+1 digits
		int[] temp = new int[size];
		int i = n - 1, j = m - 1, k = size - 1, carry = 0;

		while (i >= 0 || j >= 0 || carry != 0) {
			int sum = carry;
			if (i >= 0) sum += a[i--];
			if (j >= 0) sum += b[j--];

			temp[k--] = sum % 10;
			carry = sum / 10;
		}

		// Strip leading zero if the extra slot wasn't used (no leading zeros allowed)
		if (temp[0] == 0 && size > 1) {
			int[] arr = new int[size - 1];
			System.arraycopy(temp, 1, arr, 0, size - 1);
			return arr;
		}
		return temp;
	}
}
```
Time complexity - O(n + m), single pass over both arrays
Space complexity - O(max(n, m)) output array only, no extra auxiliary space
