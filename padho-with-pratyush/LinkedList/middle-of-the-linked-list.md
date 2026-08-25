Prob: https://leetcode.com/problems/middle-of-the-linked-list/description/

## Problem Summary
Given the head of a singly linked list, return the middle node. If there are two middle nodes, return the second one.

Sol 1: Brute Force - Two passes (count then walk)
1. First pass: walk the list to count how many nodes `n` there are.
2. Second pass: advance `n/2` steps from the head. The node you land on is the middle — and because integer division gives `n/2`, the "second middle" is returned automatically when `n` is even.

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
    public ListNode middleNode(ListNode head) {
        int n = 0;
        ListNode cur = head;
        while (cur != null) { n++; cur = cur.next; }

        cur = head;
        for (int i = 0; i < n / 2; i++) cur = cur.next;
        return cur;
    }
}
```
Time Complexity: O(n), two separate passes over the list.
Space Complexity: O(1), only a couple of pointer variables.

Sol 2: Optimal - Floyd's Tortoise and Hare (one pass)
# Intuition
Send two pointers at different speeds: slow moves 1 node per step, fast moves 2. After `k` iterations, slow has covered `k` nodes and fast has covered `2k`. So when fast reaches (or passes) the end — having traveled twice the distance — slow has covered exactly half the list, landing on the middle node. With an even count, fast ends at null after `n/2` moves while slow sits at node `n/2` (the second middle); with an odd count, fast ends on the last node and slow is the true middle.

1. Start slow and fast both at head.
2. While fast and fast.next are non-null: advance slow one step, advance fast two steps.
3. When the loop exits (fast is null or fast.next is null), slow is at the middle node — return it.

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
    public ListNode middleNode(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;

        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next;
            if (fast != null) fast = fast.next;
        }
        return slow;
    }
}
```
Time Complexity: O(n), single pass; fast reaches the end after at most n/2 steps.
Space Complexity: O(1).

## Key Takeaways
- "Find the middle in one pass" -> slow/fast pointers; fast travels twice as far, so slow is always at the midpoint.
- The `fast != null && fast.next != null` condition cleanly handles both even and odd lengths: even ends with fast == null (second middle returned), odd ends with fast.next == null (true middle returned).
- This is the same tortoise-and-hare skeleton used for Linked List Cycle and Linked List Cycle II — just without the meeting check.
- Recognition cue: "middle of a linked list", "find midpoint", or any "process only half the list" ask -> think two-speed pointers before counting.
