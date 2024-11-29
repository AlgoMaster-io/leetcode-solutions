# 731. [My Calendar II](https://leetcode.com/problems/my-calendar-ii/)

## Approach: Line Sweeping Using TreeMap

### Solution
```go
// Time Complexity: O(n^2), where n is the number of events booked
// Space Complexity: O(n), where n is the number of events stored in the map
package main

import "sort"

type MyCalendarTwo struct {
	events map[int]int
	keys   []int
}

func Constructor() MyCalendarTwo {
	return MyCalendarTwo{events: make(map[int]int)}
}

func (this *MyCalendarTwo) Book(start int, end int) bool {
	// Increment count at the start of the event
	this.events[start]++
	// Decrement count at the end of the event
	this.events[end]--

	this.keys = extractSortedKeys(this.events)

	active := 0 // Tracks active bookings at any time
	for _, k := range this.keys {
		active += this.events[k]
		if active >= 3 {
			// Triple booking found, revert changes and return false
			this.events[start]--
			if this.events[start] == 0 {
				delete(this.events, start)
			}
			this.events[end]++
			if this.events[end] == 0 {
				delete(this.events, end)
			}
			this.keys = extractSortedKeys(this.events) // Recompute keys after modification
			return false
		}
	}

	return true // No triple booking, booking succeeds
}

func extractSortedKeys(events map[int]int) []int {
	keys := make([]int, 0, len(events))
	for k := range events {
		keys = append(keys, k)
	}
	sort.Ints(keys) // Sort keys to maintain the event order
	return keys
}
```

