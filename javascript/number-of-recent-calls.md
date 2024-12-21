# [Leetcode 933: Number of Recent Calls](https://leetcode.com/problems/number-of-recent-calls/)

## Approaches:

1. [Queue for Sliding Window](#queue-for-sliding-window)

### Queue for Sliding Window

#### Intuition:
To solve the problem of efficiently counting how many requests have been received in the past 3000 milliseconds, we can use a queue data structure. This approach leverages the idea of a sliding window, where we only keep track of the timestamps that fall within the desired time window (t-3000 to t, inclusive).

When a new request comes in at time `t`, all timestamps in the queue that are less than `t-3000` can be removed, as they do not fall within the current sliding window. By maintaining only the relevant timestamps in the queue, we can easily count the number of requests within the recent 3000 milliseconds.

#### Implementation:

```javascript
var RecentCounter = function() {
    // Initialize an empty queue to store the timestamps of requests
    this.queue = [];
};

/** 
 * @param {number} t
 * @return {number}
 */
RecentCounter.prototype.ping = function(t) {
    // Add the new request timestamp to the queue
    this.queue.push(t);

    // Remove the timestamps that are out of the 3000ms range window (t-3000)
    while (this.queue[0] < t - 3000) {
        this.queue.shift();  // Remove the oldest timestamp
    }
    
    // The number of timestamps remaining in the queue is the count of recent requests
    return this.queue.length;
};
```

#### Detailed Steps:
1. **Initialization**: A `RecentCounter` object initializes an empty array `queue` to store the timestamps of the incoming requests.

2. **Adding a New Timestamp**: 
   - On each call to `ping(t)`, insert the current timestamp `t` into the queue.

3. **Maintaining the Sliding Window**:
   - While the first element of the queue (`this.queue[0]`) is less than `t - 3000`, it means the timestamp is outside the 3000-millisecond window. Therefore, remove it by calling `shift()`.

4. **Return the Count**:
   - After cleaning up old entries, the length of the queue gives the count of requests made during the past 3000 milliseconds.

#### Time and Space Complexity:
- **Time Complexity**: 
  - Each `ping` operation costs O(N), where N is the number of timestamps that need to be removed. However, on average, each entry in the queue is processed once, resulting in an amortized O(1) time complexity.
- **Space Complexity**: 
  - O(N), where N is the number of requests in the queue that have occurred in the past 3000 milliseconds. In the worst-case scenario, the queue may hold up to 3000 timestamps simultaneously.

