# 981. [Time Based Key-Value Store](https://leetcode.com/problems/time-based-key-value-store/)

## Approach: HashMap with Binary Search

### Solution
csharp
```csharp
// Time Complexity:
//   - set: O(1)
//   - get: O(log(n)), where n is the number of timestamps for a given key
// Space Complexity: O(n), where n is the total number of key-timestamp-value entries
using System;
using System.Collections.Generic;

public class TimeMap {
    private Dictionary<string, List<Pair>> map;

    public TimeMap() {
        map = new Dictionary<string, List<Pair>>();
    }

    public void Set(string key, string value, int timestamp) {
        if (!map.ContainsKey(key)) {
            map[key] = new List<Pair>();
        }
        map[key].Add(new Pair(timestamp, value));
    }

    public string Get(string key, int timestamp) {
        if (!map.ContainsKey(key)) {
            return "";
        }

        List<Pair> pairs = map[key];
        int left = 0, right = pairs.Count - 1;

        // Binary search to find the largest timestamp <= given timestamp
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (pairs[mid].Timestamp <= timestamp) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }

        // If no valid timestamp is found, return ""
        return right >= 0 ? pairs[right].Value : "";
    }

    private class Pair {
        public int Timestamp { get; }
        public string Value { get; }

        public Pair(int timestamp, string value) {
            this.Timestamp = timestamp;
            this.Value = value;
        }
    }
}

/**
 * Your TimeMap object will be instantiated and called as such:
 * TimeMap obj = new TimeMap();
 * obj.Set(key, value, timestamp);
 * string param_2 = obj.Get(key, timestamp);
 */
```

