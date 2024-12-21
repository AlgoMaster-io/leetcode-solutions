# [Leetcode 933: Number of Recent Calls](https://leetcode.com/problems/number-of-recent-calls/)

## Approaches
1. [Basic Approach: Naive Iteration](#basic-approach)
2. [Optimized Approach: Using Queue](#optimized-approach)

### Basic Approach: Naive Iteration

#### Intuition
In this approach, we will maintain a list of all the calls and for every new call, we will count how many calls have occurred in the last 3000 milliseconds. We will iterate over the list to count and return the number of recent calls.

#### Implementation

```csharp
public class RecentCounter {
    private List<int> calls;

    public RecentCounter() {
        calls = new List<int>();
    }
    
    public int Ping(int t) {
        // Add the call to the list
        calls.Add(t);
        int count = 0;
        
        // Iterate over all the calls to count how many fall within the recent 3000 ms
        foreach (int call in calls) {
            if (call >= t - 3000) {
                count++;
            }
        }
        
        // Return the number of recent calls
        return count;
    }
}
```

#### Time Complexity
- Each call to `Ping()` takes `O(n)` time where `n` is the number of total calls made so far because we iterate over all calls each time.

#### Space Complexity
- `O(n)`: We store every call made in the list.

### Optimized Approach: Using Queue

#### Intuition
Instead of storing all previous calls indefinitely, which is inefficient, we can use a queue data structure to store only those calls that are within the last 3000 milliseconds. As we add a new call time to the queue, we will also remove from the front any calls that occurred before `t - 3000`. This ensures that our queue always contains the number of recent calls within a 3000ms window.

#### Implementation

```csharp
public class RecentCounter {
    private Queue<int> queue;

    public RecentCounter() {
        queue = new Queue<int>();
    }
    
    public int Ping(int t) {
        // Add the new call time to the queue
        queue.Enqueue(t);
        
        // Dequeue calls that are earlier than t - 3000
        while (queue.Count > 0 && queue.Peek() < t - 3000) {
            queue.Dequeue();
        }
        
        // The size of the queue is the number of recent calls
        return queue.Count;
    }
}
```

#### Time Complexity
- Each call to `Ping()` takes `O(1)` on average because for each new call, old calls are dequeued only once.

#### Space Complexity
- `O(n)`: At most, the queue will contain all calls within the recent 3000 milliseconds window. In the worst-case scenario, all calls fall within this window.

