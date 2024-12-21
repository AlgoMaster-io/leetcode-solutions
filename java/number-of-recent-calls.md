# [LeetCode 933: Number of Recent Calls](https://leetcode.com/problems/number-of-recent-calls/)

## Approaches:
- [Approach 1: Brute Force with List](#approach-1-brute-force-with-list)
- [Approach 2: Optimized with Queue](#approach-2-optimized-with-queue)

### Approach 1: Brute Force with List

**Intuition**:  
The problem requires us to count the number of calls within a sliding window of 3000 milliseconds. One straightforward approach is to maintain a list of timestamps and iterate over them to count how many fall within this range each time a new call is added.

**Steps**:
1. Maintain a list to store each call received in increasing order of time.
2. On each call to `ping`, calculate the time window `[t - 3000, t]`.
3. Iterate over the list and count how many timestamps fall within this window.

**Time Complexity**:  
- Every call takes `O(n)` where `n` is the number of timestamps stored. This is because we may need to iterate over the list to count.

**Space Complexity**:  
- `O(n)` since we need to store all the timestamps.

```java
import java.util.ArrayList;

public class RecentCounter {
    private ArrayList<Integer> calls;

    public RecentCounter() {
        // Initialize the list to store timestamps
        calls = new ArrayList<>();
    }

    public int ping(int t) {
        calls.add(t); // Add the current timestamp
        // Define the time window
        int start = t - 3000; 
        int count = 0;
        // Count how many timestamps fall in this window
        for (int time : calls) { 
            if (time >= start && time <= t) {
                count++;
            }
        }
        return count;
    }
}
```

### Approach 2: Optimized with Queue

**Intuition**:  
Instead of iterating over all past calls, we can use a queue to efficiently add new timestamps and remove old ones that fall out of the sliding window, maintaining only the relevant timestamps within the queue.

**Steps**:
1. Use a queue to store timestamps.
2. On each call to `ping`, add the current timestamp to the queue.
3. Remove timestamps from the front of the queue if they are older than the start of the sliding window `[t - 3000]`.
4. The size of the queue at any time will give the count of timestamps in the required range.

**Time Complexity**:  
- Each `ping` operation is `O(1)` on average since we are performing operations that affect only the current and the oldest timestamps.

**Space Complexity**:  
- `O(n)` where `n` is the number of timestamps in the last 3000 milliseconds.

```java
import java.util.LinkedList;
import java.util.Queue;

public class RecentCounter {
    private Queue<Integer> calls;

    public RecentCounter() {
        // Initialize the queue to store timestamps
        calls = new LinkedList<>();
    }

    public int ping(int t) {
        // Add current timestamp to the queue
        calls.offer(t);
        // Remove timestamps older than (t - 3000)
        while (calls.peek() < t - 3000) {
            calls.poll();
        }
        // The size of the queue represents the number of valid recent calls
        return calls.size();
    }
}
```

This optimized solution uses a queue to efficiently manage and count the number of valid timestamps, leveraging the fact that the order of timestamps is naturally maintained by the queue, allowing for straightforward enqueue and dequeue operations.

