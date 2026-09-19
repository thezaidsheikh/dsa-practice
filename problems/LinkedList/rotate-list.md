# Rotate List

Prob: https://leetcode.com/problems/rotate-list/

## Problem Summary
Given the head of a linked list, rotate the list to the right by k places. Each rotation moves the last node to the front, and k may be much larger than the list length.

Sol 1: Brute Force - Rotate one node at a time
1. For each of the k rotations, detach the last node and place it at the front.
2. Find the last node and its previous node, relink `prev.next = null` and point the last node to the old head.
3. Repeat k times.
4. If k is larger than the size, the same work repeats — wastefully.

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
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null || head.next == null) return head;

        for (int i = 0; i < k; i++) {
            ListNode cur = head;
            while (cur.next.next != null) cur = cur.next;
            ListNode last = cur.next;
            cur.next = null;
            last.next = head;
            head = last;
        }
        return head;
    }
}
```
Time complexity - O(n * k), one full pass per rotation
Space complexity - O(1), only pointers used

Sol 2: Optimal - Linked the list, then break at the right cut
# Intuition
Rotating right by k is the same as rotating by `k % size`, because after `size` rotations the list is back to its original order. Once `k` is normalized, the new last node is the node at position `size - k - 1` (0-indexed) from the head. Linking the last node to the head turns the list into a cycle; cutting the `next` of the new last node unwinds it at exactly the rotated position.

1. If head is null, return it directly.
2. Traverse to the last node while counting size; keep `last` (the current tail).
3. Normalize k with `k = k % size`; if k becomes 0, the list is unchanged — return head.
4. Walk `size - k - 1` steps from head to find `newLast` (the node that will become the tail).
5. Link `last.next = head` to close the cycle.
6. Set `head = newLast.next` (the new head) and cut the cycle with `newLast.next = null`.
7. Return the new head.

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
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null) return head;
        int size = 1;
        ListNode last = head;

        while (last.next != null) {
            last = last.next;
            size++;
        }

        k = k % size;
        if (k == 0) return head;

        int tillNode = size - k - 1;
        ListNode newLast = head;
        while (tillNode-- != 0) {
            newLast = newLast.next;
        }

        last.next = head;
        head = newLast.next;
        newLast.next = null;

        return head;
    }
}
```
Time complexity - O(n), one pass to find the tail plus one walk to the cut point
Space complexity - O(1), only pointers used

## Key Takeaways
- Rotation is circular: `k % size` removes redundant full-cycle rotations, and `k == 0` short-circuits entirely.
- The cut point for a right-rotation by k is `size - k - 1` — the node whose `next` becomes the new head.
- Closing the list into a cycle (`last.next = head`) then breaking it at the cut point is the clean in-place way to avoid moving nodes.
- Recognition cue: "rotate the list to the right / left by k" -> normalize k, find the tail, relink.