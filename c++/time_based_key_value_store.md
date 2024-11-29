# 981. [Time Based Key-Value Store](https://leetcode.com/problems/time-based-key-value-store/)

## Approach: HashMap with Binary Search

### Solution

```cpp
// Time Complexity:
//   - set: O(1)
//   - get: O(log(n)), where n is the number of timestamps for a given key
// Space Complexity: O(n), where n is the total number of key-timestamp-value entries
#include <unordered_map>
#include <vector>
#include <string>
#include <utility>

using namespace std;

class TimeMap {
private:
    unordered_map<string, vector<pair<int, string>>> map;
    
public:
    TimeMap() {
        // Constructor
    }

    void set(string key, string value, int timestamp) {
        map[key].emplace_back(timestamp, value);
    }

    string get(string key, int timestamp) {
        if (map.find(key) == map.end()) {
            return "";
        }

        const auto& pairs = map[key];
        int left = 0, right = pairs.size() - 1;

        // Binary search to find the largest timestamp <= given timestamp
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (pairs[mid].first <= timestamp) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }

        // If no valid timestamp is found, return ""
        return right >= 0 ? pairs[right].second : "";
    }
};

/**
 * Your TimeMap object will be instantiated and called as such:
 * TimeMap* obj = new TimeMap();
 * obj->set(key,value,timestamp);
 * string param_2 = obj->get(key,timestamp);
 */
```

