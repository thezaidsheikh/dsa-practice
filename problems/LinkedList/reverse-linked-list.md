# Reverse Linked List

Prob: https://leetcode.com/problems/reverse-linked-list/

## Problem Summary
Given the head of a singly linked list, reverse the list and return the new head.

Sol 1: Brute Force - Rebuild using extra memory
1. Traverse the list and collect every value into an array (or stack).
2. Traverse the list again, writing values back from the end to the start.
3. Return the original head — it now points to a reversed list.

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
    public ListNode reverseList(ListNode head) {
        java.util.List<Integer> vals = new java.util.ArrayList<>();
        for (ListNode cur = head; cur != null; cur = cur.next) {
            vals.add(cur.val);
        }
        ListNode cur = head;
        for (int i = vals.size() - 1; i >= 0; i--) {
            cur.val = vals.get(i);
            cur = cur.next;
        }
        return head;
    }
}
```
Time complexity - O(n), two passes
Space complexity - O(n), for the stored values

Sol 2: Better - Recursion with a helper
1. Recursively go to the last node, which becomes the new head.
2. On the way back, for each node set next.next to point back to itself.
3. Break the old forward link to avoid a cycle.
4. Return the new head from every level.

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) return head;
        ListNode newHead = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return newHead;
    }
}
```
Time complexity - O(n), visits each node once
Space complexity - O(n), recursion stack depth

Sol 3: Optimal - Iterative three-pointer reversal
# Intuition
Reversing in place only needs to redirect each node's next pointer to the previous node. Three variables — prev, current, temp — let us flip the link before moving forward, so the original next node isn't lost. When current reaches null, prev is the new head.

1. Initialize prev to null and current to head.
2. While current is not null, save current.next in temp.
3. Point current.next back to prev.
4. Move prev up to current and current up to temp.
5. When the loop ends, prev is the reversed list's head.

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
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode current = head;
        while (current != null) {
            ListNode temp = current.next;
            current.next = prev;
            prev = current;
            current = temp;
        }
        return prev;
    }
}
```
Time complexity - O(n), single pass
Space complexity - O(1), only three pointers used