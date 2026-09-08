# Attend All Meetings II (Minimum Meeting Rooms)

Prob: https://www.geeksforgeeks.org/problems/attend-all-meetings-ii/1

Sol 1: Brute Force - Check overlap count per meeting
1. For each meeting, count how many other meetings overlap with it.
2. The maximum overlap count across all meetings is the number of rooms needed.
3. Two meetings overlap if start[i] < end[j] && start[j] < end[i].

```java
class Solution {
    public int minMeetingRooms(int[] start, int[] end) {
        int n = start.length;
        int maxRooms = 0;
        for (int i = 0; i < n; i++) {
            int overlap = 0;
            for (int j = 0; j < n; j++) {
                if (start[j] < end[i] && start[i] < end[j]) overlap++;
            }
            maxRooms = Math.max(maxRooms, overlap);
        }
        return maxRooms;
    }
}
```
Time complexity - O(n^2),
Space complexity - O(1)

Sol 2: Better - Sort by start, use a min-heap of end times
1. Sort meetings by start time.
2. Maintain a min-heap of end times of meetings currently using a room.
3. For each meeting, remove from the heap all meetings ending before (or at) this meeting's start.
4. Push the current meeting's end into the heap.
5. The heap size at any point is the number of rooms in use — track the max.

```java
import java.util.PriorityQueue;

class Solution {
    public int minMeetingRooms(int[] start, int[] end) {
        int n = start.length;
        int[][] meetings = new int[n][2];
        for (int i = 0; i < n; i++) {
            meetings[i][0] = start[i];
            meetings[i][1] = end[i];
        }
        Arrays.sort(meetings, (a, b) -> a[0] - b[0]);

        PriorityQueue<Integer> minHeap = new PriorityQueue<>();
        int maxRooms = 0;
        for (int[] meeting : meetings) {
            while (!minHeap.isEmpty() && minHeap.peek() <= meeting[0]) {
                minHeap.poll();
            }
            minHeap.add(meeting[1]);
            maxRooms = Math.max(maxRooms, minHeap.size());
        }
        return maxRooms;
    }
}
```
Time complexity - O(n log n),
Space complexity - O(n)

Sol 3: Optimal - Two pointers on sorted start and end arrays
# Intuition
When a meeting's start time is less than the earliest ending meeting, we must open a new room. When a meeting has already ended, its room frees up. By sorting all start and end times separately, a single sweep with two pointers tells us how many rooms are simultaneously occupied — the peak is the answer.

1. Sort the start and end arrays independently.
2. Use i to scan start times and j to scan end times.
3. If start[i] < end[j], a meeting begins before the earliest ends — increment the room count.
4. Otherwise a meeting has ended — decrement the room count.
5. Track the maximum room count seen at any point.

```java
class Solution {
    public int minMeetingRooms(int[] start, int[] end) {
        Arrays.sort(start);
        Arrays.sort(end);

        int room = 0;
        int res = 0;
        int i = 0;
        int j = 0;
        while (i < start.length && j < end.length) {
            if (start[i] < end[j]) {
                room++;
                i++;
            } else {
                room--;
                j++;
            }
            res = Math.max(res, room);
        }
        return res;
    }
}
```
Time complexity - O(n log n), dominated by sorting
Space complexity - O(1), no extra space used