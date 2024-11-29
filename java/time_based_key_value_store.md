# 981. [Time Based Key-Value Store](https://leetcode.com/problems/time-based-key-value-store/)

## Approach: HashMap with Binary Search

### Solution
```java
// Time Complexity:
//   - set: O(1)
//   - get: O(log(n)), where n is the number of timestamps for a given key
// Space Complexity: O(n), where n is the total number of key-timestamp-value entries
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;

class TimeMap {
    private HashMap<String, List<Pair>> map;

    public TimeMap() {
        map = new HashMap<>();
    }

    public void set(String key, String value, int timestamp) {
        map.putIfAbsent(key, new ArrayList<>());
        map.get(key).add(new Pair(timestamp, value));
    }

    public String get(String key, int timestamp) {
        if (!map.containsKey(key)) {
            return "";
        }

        List<Pair> pairs = map.get(key);
        int left = 0, right = pairs.size() - 1;

        // Binary search to find the largest timestamp <= given timestamp
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (pairs.get(mid).timestamp <= timestamp) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }

        // If no valid timestamp is found, return ""
        return right >= 0 ? pairs.get(right).value : "";
    }

    private static class Pair {
        int timestamp;
        String value;

        Pair(int timestamp, String value) {
            this.timestamp = timestamp;
            this.value = value;
        }
    }
}

/**
 * Your TimeMap object will be instantiated and called as such:
 * TimeMap obj = new TimeMap();
 * obj.set(key,value,timestamp);
 * String param_2 = obj.get(key,timestamp);
 */
```