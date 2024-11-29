# 981. [Time Based Key-Value Store](https://leetcode.com/problems/time-based-key-value-store/)

## Approach: HashMap with Binary Search

### Solution
```go
// Time Complexity:
//   - set: O(1)
//   - get: O(log(n)), where n is the number of timestamps for a given key
// Space Complexity: O(n), where n is the total number of key-timestamp-value entries

package main

import (
	"sort"
)

type Pair struct {
	timestamp int
	value     string
}

type TimeMap struct {
	store map[string][]Pair
}

func Constructor() TimeMap {
	return TimeMap{store: make(map[string][]Pair)}
}

func (this *TimeMap) Set(key string, value string, timestamp int) {
	this.store[key] = append(this.store[key], Pair{timestamp: timestamp, value: value})
}

func (this *TimeMap) Get(key string, timestamp int) string {
	pairs, exists := this.store[key]
	if !exists {
		return ""
	}

	// Binary search to find the largest timestamp <= given timestamp
	left, right := 0, len(pairs)-1
	for left <= right {
		mid := left + (right-left)/2
		if pairs[mid].timestamp <= timestamp {
			left = mid + 1
		} else {
			right = mid - 1
		}
	}

	// If no valid timestamp is found, return ""
	if right >= 0 {
		return pairs[right].value
	} else {
		return ""
	}
}

/**
 * Your TimeMap object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Set(key,value,timestamp);
 * param_2 := obj.Get(key,timestamp);
 */
```

