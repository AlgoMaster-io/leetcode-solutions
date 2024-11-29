# [933. Number of Recent Calls](https://leetcode.com/problems/number-of-recent-calls/)

## Approach 1: Brute Force

### Solution
csharp
```csharp
// Time Complexity: O(n) per request (where n is the number of calls in the time window)
// Space Complexity: O(n)
using System.Collections.Generic;

public class RecentCounter {
    private List<int> requests;

    public RecentCounter() {
        requests = new List<int>();
    }

    public int Ping(int t) {
        requests.Add(t); // Add the new request
        int count = 0;

        // Count all requests in the range [t - 3000, t]
        foreach (int time in requests) {
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
csharp
```csharp
// Time Complexity: O(1) per request (amortized)
// Space Complexity: O(n)
using System.Collections.Generic;

public class RecentCounter {
    private Queue<int> queue;

    public RecentCounter() {
        queue = new Queue<int>();
    }

    public int Ping(int t) {
        queue.Enqueue(t); // Add the new request to the queue

        // Remove requests that are outside the range [t - 3000, t]
        while (queue.Count > 0 && queue.Peek() < t - 3000) {
            queue.Dequeue();
        }

        return queue.Count; // The size of the queue represents the number of valid requests
    }
}
```

