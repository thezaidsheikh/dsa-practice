# Reverse Linked List II

Prob: https://leetcode.com/problems/reverse-linked-list-ii/description/

## Problem Summary
Given the head of a singly linked list and two integers left and right (1-indexed), reverse the nodes from position left to position right and return the list.

Sol 1: Brute Force - Extract values and write back
1. Walk the list, collecting values into an array (or list).
2. Reverse the subarray between indices left - 1 and right - 1.
3. Walk the list again, writing the reversed values back between the two positions.

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
    public ListNode reverseBetween(ListNode head, int left, int right) {
        java.util.List<Integer> vals = new java.util.ArrayList<>();
        for (ListNode cur = head; cur != null; cur = cur.next) {
            vals.add(cur.val);
        }
        while (left < right) {
            int temp = vals.get(left - 1);
            vals.set(left - 1, vals.get(right - 1));
            vals.set(right - 1, temp);
            left++;
            right--;
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

Sol 2: Better - Split list, reverse middle, reconnect
1. Isolate the segment between left and right by tracking the node before the segment.
2. Detach/reverse only that middle segment.
3. Reconnect the reversed segment with the node before and the node after.
4. Return the original head, or the new head if left == 1.

```java
class Solution {
    public ListNode reverseBetween(ListNode head, int left, int right) {
        ListNode dummy = new ListNode(0, head);
        ListNode before = dummy;
        for (int i = 1; i < left; i++) before = before.next;

        ListNode prev = null;
        ListNode curr = before.next;
        for (int i = left; i <= right; i++) {
            ListNode temp = curr.next;
            curr.next = prev;
            prev = curr;
            curr = temp;
        }

        before.next.next = curr;
        before.next = prev;
        return dummy.next;
    }
}
```
Time complexity - O(right), only the segment is reversed
Space complexity - O(1), dummy node only

Sol 3: Optimal - Single-pass in-place reversal
# Intuition
Instead of isolating the segment first, walk the list once: move before up to the node just before left, then reverse links with prev/curr like in plain reverse-list, and finally reconnect the segment's first node to the node after right. Two pointers (before and firstNode) remember where the reversed part begins and how to hook it back.

1. If right - left == 0, nothing to reverse — return head.
2. Detect whether left is at the head (counter 1); that decides the final return node.
3. Walk counter from 1 to right.
4. When counter < left, keep advancing before.
5. When counter >= left, reverse the links using prev and curr.
6. After the loop, firstNode.next = curr reconnects the tail.
7. If left > 1, before.next = prev and return head; otherwise return prev as the new head.

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
    public ListNode reverseBetween(ListNode head, int left, int right) {
        if (right - left == 0) return head;

        ListNode before = null;
        ListNode firstNode = null;
        ListNode prev = null;
        ListNode curr = head;
        int counter = 1;
        boolean isFromStarting = false;
        if (left == counter) isFromStarting = true;

        while (counter <= right) {
            if (counter == left) firstNode = curr;
            if (counter < left) {
                before = curr;
                curr = curr.next;
            } else {
                ListNode temp = curr.next;
                curr.next = prev;
                prev = curr;
                curr = temp;
            }
            counter++;
        }

        firstNode.next = curr;
        if (!isFromStarting) {
            before.next = prev;
            return head;
        } else {
            return prev;
        }
    }
}
```
Time complexity - O(n), single pass up to right
Space complexity - O(1), only a few pointers used