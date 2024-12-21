# [LeetCode 981: Time Based Key-Value Store](https://leetcode.com/problems/time-based-key-value-store/)

## Approaches

1. [Brute Force using Linear Search](#1-brute-force-using-linear-search)
2. [Optimized using Binary Search](#2-optimized-using-binary-search)

---

### 1. Brute Force using Linear Search

#### Intuition

The idea is to store each key and its respective values alongside the timestamp when it was set. When a `get` query is made, iterate through the stored timestamps and values to find the value with the greatest timestamp less than or equal to the queried one. Despite being straightforward, for large datasets, this approach could prove to be inefficient due to the linear scan.

#### Solution

```javascript
class TimeMap {
    constructor() {
        this.store = {}; // A map to store keys and their associated value-timestamp pairs.
    }
    
    // Sets the (key, value) with the given timestamp
    set(key, value, timestamp) {
        if (!this.store[key]) {
            this.store[key] = []; // Initialize if key doesn't exist
        }
        // Push the value and timestamp as a pair
        this.store[key].push([value, timestamp]);
    }
    
    // Gets the value for key with the largest timestamp <= given timestamp
    get(key, timestamp) {
        if (!this.store[key]) {
            return ""; // Return empty if key doesn't exist
        }
        
        let value = "";
        // Iterate over the stored pairs to find suitable value
        for (let [val, time] of this.store[key]) {
            if (time <= timestamp) {
                value = val;
            } else {
                break; // Since timestamps are sorted when added
            }
        }
        return value;
    }
}
```

#### Complexity

- **Time Complexity**: 
  - `set`: \(O(1)\) because we simply append the value.
  - `get`: \(O(N)\) where \(N\) is the number of timestamps associated with the key.
- **Space Complexity**: \(O(N)\) where \(N\) is the total number of `set` operations.

---

### 2. Optimized using Binary Search

#### Intuition

To improve the efficiency of searching the maximum possible timestamp less than or equal to the provided timestamp, a binary search can be leveraged. By keeping the timestamps sorted, one can significantly reduce the retrieval time. The plan is to store the values along with their timestamps and perform a binary search on these timestamps for each `get` operation.

#### Solution

```javascript
class TimeMap {
    constructor() {
        this.store = {}; // A map to store keys and associated value-timestamp pairs.
    }
  
    set(key, value, timestamp) {
        if (!this.store[key]) {
            this.store[key] = []; // Initialize if the key doesn't exist
        }
        this.store[key].push([value, timestamp]); // Append the value and timestamp
    }
  
    get(key, timestamp) {
        if (!this.store[key]) return ""; // Return empty if key is not present
        
        const pairs = this.store[key];
        let left = 0;
        let right = pairs.length - 1;
        let result = "";
        
        // Binary search for the timestamp
        while (left <= right) {
            const mid = Math.floor((left + right) / 2);
            if (pairs[mid][1] <= timestamp) {
                result = pairs[mid][0];
                left = mid + 1; // Look on the right part
            } else {
                right = mid - 1; // Look on the left part
            }
        }
        
        return result;
    }
}
```

#### Complexity

- **Time Complexity**: 
  - `set`: \(O(1)\) because we just append the value.
  - `get`: \(O(\log N)\) where \(N\) is the number of timestamps for the key due to binary search.
- **Space Complexity**: \(O(N)\) where \(N\) is the total number of `set` operations.

These solutions allow the `TimeMap` to efficiently store and query historical data with varying levels of optimization based on the approach chosen.

