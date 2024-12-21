# [Leetcode 981: Time Based Key-Value Store](https://leetcode.com/problems/time-based-key-value-store/)

## Approaches
1. [Basic Approach: Brute Force Linear Search](#basic-approach)
2. [Optimized Approach: Binary Search on Timestamps](#binary-search-approach)

---

## Basic Approach: Brute Force Linear Search

### Intuition

We need a `TimeMap` class to store and retrieve key-value pairs associated with timestamps in increasing order. The simplest way to organize this data is to store all values associated with a key in a list. When retrieving the value for a specific key and timestamp, we can iterate through all the timestamps for the key, accumulating values that are valid less than or equal to the requested timestamp, and finally return the latest valid entry from the accumulated values.

### Code

```typescript
class TimeMap {
    private map: Map<string, [number, string][]>;

    constructor() {
        // Initialize a map to store key values along with their timestamp
        this.map = new Map();
    }

    set(key: string, value: string, timestamp: number): void {
        // If the key doesn't exist in the map, initialize it with an empty array
        if (!this.map.has(key)) {
            this.map.set(key, []);
        }
        // Append the timestamped value to the list of timestamps for the associated key
        this.map.get(key).push([timestamp, value]);
    }

    get(key: string, timestamp: number): string {
        const timeEntries = this.map.get(key) || [];
        let result = "";
        // Go through the stored timestamps until finding a suitable one
        for (let [time, value] of timeEntries) {
            if (time <= timestamp) {
                result = value; // update result as long as timestamps are valid
            } else {
                break; // stop searching if future timestamps are found
            }
        }
        return result;
    }
}
```

### Time Complexity

- `set`: O(1) - Adding a new entry is a constant time operation.
- `get`: O(n) - In the worst case, we might have to examine all the entries that existed before or at the requested timestamp.

### Space Complexity

- O(n) - We store every entry with its associated timestamp and value, where n is the number of entries set.

---

## Optimized Approach: Binary Search on Timestamps

### Intuition

Instead of linearly scanning through all timestamps for a key in the `get` operation, we can use binary search to locate the maximum valid timestamp. This requires the list of timestamps to be sorted, which is inherently maintained if we keep appending to the list in increasing time order. This will allow for faster `get` calls.

### Code

```typescript
class TimeMap {
    private map: Map<string, [number, string][]>;

    constructor() {
        this.map = new Map();
    }

    set(key: string, value: string, timestamp: number): void {
        if (!this.map.has(key)) {
            this.map.set(key, []);
        }
        this.map.get(key).push([timestamp, value]);
    }

    get(key: string, timestamp: number): string {
        const timeEntries = this.map.get(key) || [];
        
        // Binary search to find the latest timestamp <= the given timestamp
        let left = 0;
        let right = timeEntries.length - 1;
        let result = "";

        while (left <= right) {
            const mid = Math.floor((left + right) / 2);
            // Check the timestamp at mid
            if (timeEntries[mid][0] <= timestamp) {
                result = timeEntries[mid][1];
                left = mid + 1; // try to find if there is a closer timestamp
            } else {
                right = mid - 1; // move left because current timestamp is too big
            }
        }

        return result;
    }
}
```

### Time Complexity

- `set`: O(1) - Adding a new entry continues to be a constant time operation.
- `get`: O(log n) - Performing a binary search over the list of timestamps for a given key.

### Space Complexity

- O(n) - Similar space requirements as before, simply storing all entries.

By employing binary search, this approach significantly improves the efficiency of retrieval operations, particularly beneficial when there are many timestamps for a single key.

