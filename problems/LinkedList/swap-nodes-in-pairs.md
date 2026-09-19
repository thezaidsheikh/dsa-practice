# Swap Nodes in Pairs

Prob: https://leetcode.com/problems/swap-nodes-in-pairs/description/

## Problem Summary
Given the head of a linked list, swap every two adjacent nodes and return the head. Nodes are swapped, not just their values, and the original order must be preserved otherwise.

Sol 1: Brute Force - Swap values in a list
1. Collect all node values into an array.
2. Swap adjacent pairs of values in the array.
3. Write the swapped values back into the nodes.
4. Return the same head.

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
    public ListNode swapPairs(ListNode head) {
        java.util.List<Integer> vals = new java.util.ArrayList<>();
        for (ListNode cur = head; cur != null; cur = cur.next) {
            vals.add(cur.val);
        }
        for (int i = 0; i + 1 < vals.size(); i += 2) {
            int temp = vals.get(i);
            vals.set(i, vals.get(i + 1));
            vals.set(i + 1, temp);
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
1. If the list has fewer than two nodes, return head.
2. Save the second node as newHead.
3. Point head.next to the result of recursively swapping the rest (from node 3 onward).
4. Point newHead.next back to head.
5. Return newHead from each level.

```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        if (head == null || head.next == null) return head;

        ListNode newHead = head.next;
        head.next = swapPairs(head.next.next);
        newHead.next = head;
        return newHead;
    }
}
```
Time complexity - O(n), visits each node once
Space complexity - O(n), recursion stack depth

Sol 3: Optimal - Iterative with a reverse helper for each pair
# Intuition
Each pair [left, right] is a tiny sublist of two nodes that can be reversed with the standard 3-pointer trick. After reversing a pair, hook it to the previous pair's tail (prevLeft) and continue from the node after the pair (nextLeft). This swaps the actual node links in place with only constant extra space.

1. Treat the whole list as consecutive pairs starting at left.
2. For each pair, capture right (second node) and nextLeft (node after the pair).
3. Reverse exactly the two nodes with the helper (times = 2).
4. If there was a previous pair, connect its tail to right (the pair's new head).
5. Record the first swapped pair's head into res for the answer.
6. Move to the next pair from nextLeft.
7. When no pair is left, connect prevLeft to the remaining single node and break.

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
    public static void reverse(ListNode left, int times) {
        ListNode prev = null;
        ListNode cur = left;
        while (times > 0 && cur != null) {
            ListNode tmp = cur.next;
            cur.next = prev;
            prev = cur;
            cur = tmp;
            times--;
        }
    }

    public ListNode swapPairs(ListNode head) {
        ListNode left = head;
        ListNode prevLeft = null;
        ListNode nextLeft = null;
        ListNode res = null;

        while (true) {
            ListNode right = left;
            for (int i = 0; i < 1; i++) {
                if (right == null) break;
                right = right.next;
            }
            if (right != null) {
                nextLeft = right.next;
                reverse(left, 2);
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