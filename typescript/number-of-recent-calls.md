# [Leetcode 933: Number of Recent Calls](https://leetcode.com/problems/number-of-recent-calls/)

## Table of Contents
- [Approach 1: Simple Array Storage](#approach-1-simple-array-storage)
- [Approach 2: Improved with Queue (Optimal)](#approach-2-improved-with-queue-optimal)

## Approach 1: Simple Array Storage

### Intuition
The problem requires storing all the ping calls and returning the number of calls made within the last 3000 milliseconds. The simplest way to achieve this is by maintaining a list of all timestamps and checking against this list for each incoming ping.

### Steps
1. Store each timestamp in an array.
2. For each incoming ping, iterate over the array and count timestamps that fall within the range `(t - 3000)` to `t`.

### Code
```typescript
class RecentCounter {
    private timestamps: number[];

    constructor() {
        // Initialize an empty array to store timestamps
        this.timestamps = [];
    }

    ping(t: number): number {
        // Add the current timestamp to the array
        this.timestamps.push(t);

        // Count how many timestamps fall within the last 3000ms
        let count = 0;
        for (let timestamp of this.timestamps) {
            // Check if timestamp is within the range (t - 3000, t)
            if (timestamp >= t - 3000) {
                count++;
            }
        }

        return count;
    }
}

// Example usage:
const counter = new RecentCounter();
console.log(counter.ping(100));     // Output: 1
console.log(counter.ping(3001));    // Output: 2
console.log(counter.ping(3002));    // Output: 3
```

### Time and Space Complexity
- **Time Complexity**: Each call to `ping` has a time complexity of \(O(N)\), where \(N\) is the number of stored timestamps.
- **Space Complexity**: \(O(N)\), as we are storing all timestamps.

## Approach 2: Improved with Queue (Optimal)

### Intuition
The inefficiency of Approach 1 is that we're checking all timestamps every time we call `ping()`, even those that are well outside the range of interest. Using a queue allows us to process only relevant timestamps.

### Steps
1. Use a queue to maintain only the timestamps that are within the last 3000ms of the current ping.
2. For each new ping, remove all timestamps from the front of the queue that fall outside the range `(t - 3000, t)`.
3. Add the current ping timestamp to the queue.
4. The size of the queue at any time gives the number of recent calls.

### Code
```typescript
class RecentCounter {
    private queue: number[];

    constructor() {
        // Initialize an empty queue to store timestamps within 3000ms
        this.queue = [];
    }

    ping(t: number): number {
        // Add the current timestamp to the queue
        this.queue.push(t);

        // Remove timestamps that fall outside the range (t - 3000, t)
        while (this.queue[0] < t - 3000) {
            this.queue.shift();
        }
        
        // Return the number of timestamps in the queue
        return this.queue.length;
    }
}

// Example usage:
const counter = new RecentCounter();
console.log(counter.ping(100));     // Output: 1
console.log(counter.ping(3001));    // Output: 2
console.log(counter.ping(3002));    // Output: 3
```

### Time and Space Complexity
- **Time Complexity**: Average \(O(1)\) per `ping()`, since each timestamp is added and then removed at most once.
- **Space Complexity**: \(O(W)\), where \(W\) is the time window (in milliseconds) that the queue keeps (max: 3000ms).

