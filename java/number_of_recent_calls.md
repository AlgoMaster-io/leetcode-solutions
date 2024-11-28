# [933. Number of Recent Calls](https://leetcode.com/problems/number-of-recent-calls/)

## Approach 1: Brute Force

### Solution
```java
// Time Complexity: O(n) per request (where n is the number of calls in the time window)
// Space Complexity: O(n)
import java.util.ArrayList;

public class RecentCounter {
    private ArrayList<Integer> requests;

    public RecentCounter() {
        requests = new ArrayList<>();
    }

    public int ping(int t) {
        requests.add(t); // Add the new request
        int count = 0;

        // Count all requests in the range [t - 3000, t]
        for (int time : requests) {
            if (time >= t - 3000) {
                count++;
            }
        }

        return count;
    }
}
```

## Approach 2: Using a Queue (Optimal Solution)

### Solution
```java
// Time Complexity: O(1) per request (amortized)
// Space Complexity: O(n)
import java.util.LinkedList;
import java.util.Queue;

public class RecentCounter {
    private Queue<Integer> queue;

    public RecentCounter() {
        queue = new LinkedList<>();
    }

    public int ping(int t) {
        queue.add(t); // Add the new request to the queue

        // Remove requests that are outside the range [t - 3000, t]
        while (!queue.isEmpty() && queue.peek() < t - 3000) {
            queue.poll();
        }

        return queue.size(); // The size of the queue represents the number of valid requests
    }
}
```