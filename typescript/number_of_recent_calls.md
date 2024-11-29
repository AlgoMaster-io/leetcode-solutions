# [933. Number of Recent Calls](https://leetcode.com/problems/number-of-recent-calls/)

## Approach 1: Brute Force

### Solution
typescript
```typescript
// Time Complexity: O(n) per request (where n is the number of calls in the time window)
// Space Complexity: O(n)
class RecentCounter {
    private requests: number[];

    constructor() {
        this.requests = [];
    }

    ping(t: number): number {
        this.requests.push(t); // Add the new request
        let count = 0;

        // Count all requests in the range [t - 3000, t]
        for (let time of this.requests) {
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
typescript
```typescript
// Time Complexity: O(1) per request (amortized)
// Space Complexity: O(n)
class RecentCounter {
    private queue: number[];

    constructor() {
        this.queue = [];
    }

    ping(t: number): number {
        this.queue.push(t); // Add the new request to the queue

        // Remove requests that are outside the range [t - 3000, t]
        while (this.queue.length > 0 && this.queue[0] < t - 3000) {
            this.queue.shift();
        }

        return this.queue.length; // The size of the queue represents the number of valid requests
    }
}
```

