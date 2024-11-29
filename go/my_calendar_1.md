# 729. [My Calendar I](https://leetcode.com/problems/my-calendar-i/)

## Approach 1: Brute Force with List of Intervals

### Solution
go
```go
// Time Complexity: O(n^2), where n is the number of events booked
// Space Complexity: O(n), where n is the number of events stored
package mycalendar

type MyCalendar struct {
    calendar [][2]int
}

func Constructor() MyCalendar {
    return MyCalendar{calendar: make([][2]int, 0)}
}

func (this *MyCalendar) Book(start, end int) bool {
    for _, event := range this.calendar {
        // Check if the new event overlaps with any existing event
        if start < event[1] && end > event[0] {
            return false // Overlap found, booking fails
        }
    }
    this.calendar = append(this.calendar, [2]int{start, end}) // No overlap, booking succeeds
    return true
}
```

## Approach 2: TreeMap for Efficient Overlap Check

### Solution
go
```go
// Time Complexity: O(log n) per booking, where n is the number of events
// Space Complexity: O(n), where n is the number of events stored
package mycalendar

import "golang.org/x/exp/constraints"

type TreeMap[K constraints.Ordered, V any] struct {
    keys    []K
    values  map[K]V
}

// NewTreeMap creates a new tree map
func NewTreeMap[K constraints.Ordered, V any]() *TreeMap[K, V] {
    return &TreeMap[K, V]{keys: []K{}, values: make(map[K]V)}
}

// Put inserts a key-value pair into the map
func (tm *TreeMap[K, V]) Put(key K, value V) {
    tm.keys = append(tm.keys, key)
    tm.values[key] = value
}

// FloorKey returns the greatest key less than or equal to the given key, or nil
func (tm *TreeMap[K, V]) FloorKey(key K) *K {
    var best *K
    for _, k := range tm.keys {
        if k <= key && (best == nil || k > *best) {
            best = &k
        }
    }
    return best
}

// CeilingKey returns the smallest key greater than or equal to the given key, or nil
func (tm *TreeMap[K, V]) CeilingKey(key K) *K {
    var best *K
    for _, k := range tm.keys {
        if k >= key && (best == nil || k < *best) {
            best = &k
        }
    }
    return best
}

type MyCalendar struct {
    calendar *TreeMap[int, int]
}

func Constructor() MyCalendar {
    return MyCalendar{calendar: NewTreeMap[int, int]()}
}

func (this *MyCalendar) Book(start, end int) bool {
    prev := this.calendar.FloorKey(start)
    next := this.calendar.CeilingKey(start)

    if (prev == nil || this.calendar.values[*prev] <= start) &&
        (next == nil || *next >= end) {
        this.calendar.Put(start, end)
        return true
    }
    return false
}
```

