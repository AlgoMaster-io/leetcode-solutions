# 981. [Time Based Key-Value Store](https://leetcode.com/problems/time-based-key-value-store/)

## Approach: HashMap with Binary Search

### Solution
```javascript
// Time Complexity:
//   - set: O(1)
//   - get: O(log(n)), where n is the number of timestamps for a given key
// Space Complexity: O(n), where n is the total number of key-timestamp-value entries

class TimeMap {
    constructor() {
        this.map = new Map();
    }

    set(key, value, timestamp) {
        if (!this.map.has(key)) {
            this.map.set(key, []);
        }
        this.map.get(key).push({ timestamp, value });
    }

    get(key, timestamp) {
        if (!this.map.has(key)) {
            return "";
        }

        const pairs = this.map.get(key);
        let left = 0, right = pairs.length - 1;

        // Binary search to find the largest timestamp <= given timestamp
        while (left <= right) {
            const mid = left + Math.floor((right - left) / 2);
            if (pairs[mid].timestamp <= timestamp) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }

        // If no valid timestamp is found, return ""
        return right >= 0 ? pairs[right].value : "";
    }
}

/**
 * Your TimeMap object will be instantiated and called as such:
 * const obj = new TimeMap();
 * obj.set(key, value, timestamp);
 * const param_2 = obj.get(key, timestamp);
 */
```

