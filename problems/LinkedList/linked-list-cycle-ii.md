Prob: https://leetcode.com/problems/linked-list-cycle-ii/description/

## Problem Summary
Given a linked list, return the node where the cycle begins. If there is no cycle, return null. A cycle is a node that is reached again by following next pointers (the tail points back into the list).

Sol 1: Brute Force - HashSet of visited nodes
1. Walk the list one node at a time, storing every visited node object in a HashSet.
2. Before inserting, check if the current node is already present. The FIRST node that's already in the set is exactly the node where the cycle begins — it's the entry point we revisit after completing one loop of the cycle.
3. If we reach null, the list terminates: no cycle.

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {
        HashSet<ListNode> visited = new HashSet<>();
        ListNode cur = head;

        while (cur != null) {
            if (!visited.add(cur)) return cur;   // first repeat = cycle start
            cur = cur.next;
        }
        return null;
    }
}
```
Time Complexity: O(n), single pass with O(1) average set operations.
Space Complexity: O(n), for the set of visited nodes.

Sol 2: Optimal - Floyd's Tortoise and Hare (two-phase, O(1) space)
# Intuition
Phase 1 (detect): send slow and fast around the list at speeds 1 and 2. If they meet, a cycle exists; if fast hits null, there is no cycle.

Phase 2 (locate the start) is the classic math follow-up. Suppose:
- `m` = number of nodes before the cycle starts,
- `k` = distance from the cycle start to the meeting point (inside the cycle, 0-based),
- `L` = cycle length.

When they meet, slow has traveled `m + k` steps. Fast has traveled twice that, i.e. `2(m + k)`. But fast also equals slow's distance plus whole extra loops of the cycle: `2(m + k) = m + k + n*L` for some integer `n >= 1`. Rearranging gives `m + k = n*L`, so `m = n*L - k`. Equivalently `m = (n-1)*L + (L - k)`.

`L - k` is the distance from the meeting point forward to the cycle start. So if we reset one pointer to the head and advance both pointers one step at a time, the reset pointer travels `m` (the non-cycle part) while the other travels `(n-1)` full loops plus `(L - k)` to the start — both arrive at the cycle start at the SAME time. That meeting node is the answer.

1. Start slow and fast both at head. While fast and fast.next survive, move slow one step and fast two steps; if they meet, break.
2. If the loop exits without a meeting (fast hit null), return null — no cycle.
3. Reset slow to head, keep fast at the meeting point. Advance BOTH one step per iteration until they meet.
4. The meeting node is the cycle's starting node — return it.

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;

        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next;
            if (fast != null) fast = fast.next;
            if (fast == slow) {
                slow = head;
                while (slow != fast) {
                    slow = slow.next;
                    fast = fast.next;
                }
                return slow;
            }
        }
        return null;
    }
}
```
Time Complexity: O(n), both phases together traverse the list a constant number of times (at most ~3 passes).
Space Complexity: O(1), only two pointers.

## Key Takeaways
- Detecting a cycle and FINDING its start are two different asks; HashSet handles both but costs O(n) space.
- Floyd's two-phase trick finds the start in O(1) space: after the slow/fast meeting, reset one pointer to head and advance both by 1 — they meet at the cycle entry.
- The proof rests on `m = n*L - k`: the head-to-start distance equals the meeting-point-to-start distance plus whole cycle loops.
- Compare nodes (`fast == slow`), never values — duplicate values are legal in a cyclic list.
- Recognition cue: "detect/find the cycle start in O(1) space" -> tortoise and hare, then the head-reset phase.
