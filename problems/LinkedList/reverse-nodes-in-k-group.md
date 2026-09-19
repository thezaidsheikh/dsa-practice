# Reverse Nodes in K-Group

Prob: https://leetcode.com/problems/reverse-nodes-in-k-group/description/

## Problem Summary
Given the head of a linked list and an integer k, reverse the nodes of the list k at a time. If the number of nodes is not a multiple of k, the leftover nodes at the end stay in their original order.

Sol 1: Brute Force - Reverse values in a list
1. Collect all node values into an array.
2. For each block of k values, reverse the block in the array.
3. Leave any trailing values (fewer than k) untouched.
4. Write the modified values back into the nodes and return the head.

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        java.util.List<Integer> vals = new java.util.ArrayList<>();
        for (ListNode cur = head; cur != null; cur = cur.next) {
            vals.add(cur.val);
        }
        for (int i = 0; i + k <= vals.size(); i += k) {
            int lo = i, hi = i + k - 1;
            while (lo < hi) {
                int temp = vals.get(lo);
                vals.set(lo, vals.get(hi));
                vals.set(hi, temp);
                lo++;
                hi--;
            }
        }
        ListNode cur = head;
        int i = 0;
        while (cur != null) {
            cur.val = vals.get(i++);
            cur = cur.next;
        }
        return head;
    }
}
```
Time complexity - O(n), two passes
Space complexity - O(n), for the stored values

Sol 2: Better - Recursion
1. First count k nodes ahead; if fewer remain, return head unchanged.
2. Reverse the next k nodes iteratively.
3. Point the tail of the reversed block to the result of recursing on the rest.
4. Return the new head of the current block.

```java
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode node = head;
        int count = 0;
        while (count < k && node != null) {
            node = node.next;
            count++;
        }
        if (count < k) return head;

        ListNode prev = null;
        ListNode curr = head;
        for (int i = 0; i < k; i++) {
            ListNode tmp = curr.next;
            curr.next = prev;
            prev = curr;
            curr = tmp;
        }
        head.next = reverseKGroup(curr, k);
        return prev;
    }
}
```
Time complexity - O(n), visits each node once
Space complexity - O(n / k), recursion stack depth

Sol 3: Optimal - Iterative with a reverse helper per block
# Intuition
Every group of k nodes is a small sublist handled by the same 3-pointer reversal as reverse-list. After reversing a block, its new head is the block's last node (right) and its new tail is the block's first node (left). Connecting prevLeft → right and left → nextLeft stitches each reversed block back into the chain in constant space.

1. Treat the list as consecutive blocks of k nodes starting at left.
2. Walk k - 1 steps to find right (the last node of the block).
3. If right is null, fewer than k nodes remain — attach them as-is and stop.
4. Otherwise capture nextLeft (node after the block) and reverse the k nodes.
5. Connect the previous block's tail (prevLeft) to right, the block's new head.
6. Remember left as the block's new tail for the next connection.
7. Save the first block's new head into res, then continue from nextLeft.

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public void reverse(ListNode left, int k) {
        ListNode prev = null;
        ListNode curr = left;
        while (curr != null && k > 0) {
            ListNode tmp = curr.next;
            curr.next = prev;
            prev = curr;
            curr = tmp;
            k--;
        }
    }

    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode left = head;
        ListNode prevLeft = null;
        ListNode res = null;

        while (true) {
            ListNode right = left;
            for (int i = 0; i < k - 1; i++) {
                if (right == null) break;
                right = right.next;
            }

            if (right != null) {
                ListNode nextLeft = right.next;
                reverse(left, k);
                if (prevLeft != null) prevLeft.next = right;
                prevLeft = left;
                if (res == null) res = right;
                left = nextLeft;
            } else {
                if (prevLeft != null) prevLeft.next = left;
                if (res == null) res = left;
                break;
            }
        }
        return res;
    }
}
```
Time complexity - O(n), each node processed once
Space complexity - O(1), only pointers used