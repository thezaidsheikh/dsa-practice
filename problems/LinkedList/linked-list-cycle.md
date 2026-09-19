Prob: https://leetcode.com/problems/linked-list-cycle/

Sol 1: Brute Force - HashSet of visited nodes
1. Walk the list one node at a time, storing every visited node in a HashSet.
2. Before inserting, check if the current node is already present — a repeated NODE OBJECT means two paths merged, i.e., a cycle.
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
    public boolean hasCycle(ListNode head) {
        HashSet<ListNode> visited = new HashSet<>();
        ListNode cur = head;

        while (cur != null) {
            if (!visited.add(cur)) return true;
            cur = cur.next;
        }
        return false;
    }
}
```
Time Complexity: O(n), single pass with O(1) average set operations.
Space Complexity: O(n), for the set of visited nodes.

Sol 2: Better - Step-count bound (O(1) space using constraints)
# Intuition
A cycle is the ONLY way a traversal can visit more nodes than exist. If the list has n nodes (given as at most 10^4), any walk longer than n steps without hitting null proves a repeat occurred somewhere behind us.

1. Take the node-count bound from the problem constraints (10^4).
2. Walk forward, counting steps.
3. If we hit null first -> no cycle. If the counter exceeds the bound -> the walk revisited nodes -> cycle.

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        int limit = (int) 1e4 + 1;   // constraint: at most 10^4 nodes
        ListNode cur = head;
        int count = 0;

        while (cur != null && count <= limit) {
            cur = cur.next;
            count++;
        }
        return cur != null;
    }
}
```
Time Complexity: O(n)
Space Complexity: O(1)

Caveat: correctness leans on the stated constraint, not the structure of the list itself — fine when the bound is known, useless in general.

Sol 3: Optimal - Floyd's Tortoise and Hare (slow/fast pointers)
# Intuition
Send two pointers around the list at different speeds: slow advances one node per step, fast advances two. If there is no cycle, fast runs off the end and the loop exits. If there IS a cycle, both pointers eventually enter it — and once inside, fast closes the distance by exactly 1 node per iteration (relative speed 2 − 1 = 1), so it cannot jump past slow; it must land exactly on it. Meeting is therefore GUARANTEED, within at most cycle-length iterations after both enter.

1. Start slow and fast both at head.
2. While fast and fast.next survive: move slow one step, move fast two steps.
3. After moving, if fast == slow, two pointers at different speeds met -> cycle -> return true.
4. If the loop exits naturally, fast hit the end -> return false.

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;

        while(fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next;
            if(fast != null ) fast = fast.next;
            if(fast == slow) return true;
        }
        return false;
    }
}
```
Time Complexity: O(n), slow travels less than the full list before either termination or meeting.
Space Complexity: O(1)

## Key Takeaways
- Cycle detection with O(1) space = two pointers at DIFFERENT SPEEDS. The meeting guarantee comes from the relative speed being exactly 1, so fast can't leapfrog slow inside the cycle.
- Compare nodes (`fast == slow`), never values — duplicate values are legal in a cyclic list.
- The middle `if(fast != null)` guard is always true (the while condition already proved fast.next existed before the first advance); it's harmless defensive code.
- Same fast/slow skeleton solves: Linked List Cycle II (reset one pointer to head after meeting to find cycle START — classic math follow-up), Middle of the Linked List, Happy Number.
- Recognition cue: "detect repetition/cycle in O(1) space" or "find middle" -> think tortoise and hare before hash sets.
