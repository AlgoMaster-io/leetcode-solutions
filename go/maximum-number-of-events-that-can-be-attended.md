# [1353. Maximum Number of Events That Can Be Attended](https://leetcode.com/problems/maximum-number-of-events-that-can-be-attended/)

## Approaches
- [Basic Greedy Approach](#basic-greedy-approach)
- [Optimized Greedy Approach with Min-Heap](#optimized-greedy-approach-with-min-heap)

### Basic Greedy Approach

**Intuition:**

In this approach, the idea is to sort the events by their end time and iterate over them, greedily attending at the first available day which does not clash with any previously attended events. The events are sorted by end time so that we can attend the event which ends the earliest, ensuring that more time is available for future events.

**Steps:**

1. Sort the events based on their end day.
2. Use a set to keep track of the days we have already attended events.
3. Iterate over the sorted events:
   - For each event, iterate through its start day to end day.
   - If a day is available (not in the set), attend the event on that day and add this day to the set, then break out of the loop.
4. Return the count of attended events.

**Time Complexity:** \(O(n \log n)\) due to the sorting step.

**Space Complexity:** \(O(n)\) due to the space required for storing used days.

```go
func maxEvents(events [][]int) int {
    // Sort events by end day
    sort.Slice(events, func(i, j int) bool {
        return events[i][1] < events[j][1]
    })

    attendedDays := map[int]bool{}
    count := 0

    for _, event := range events {
        // Attempt to attend one day in the event's range
        for d := event[0]; d <= event[1]; d++ {
            if !attendedDays[d] {
                attendedDays[d] = true
                count++
                break
            }
        }
    }
    return count
}
```

### Optimized Greedy Approach with Min-Heap

**Intuition:**

This is an optimized solution using a min-heap (priority queue). The idea is to always attend the event with the earliest end time available. By using a min-heap, we efficiently track the current events available to attend on each day and always attend the one that ends the earliest.

**Steps:**

1. Sort events by their start day.
2. Use a min-heap to keep track of the end days of the current ongoing events.
3. Iterate over the days starting from the earliest possible event start day to the latest event end day:
   - Remove expired events from the heap (those which end before the current day).
   - Add any new events starting on the current day to the heap.
   - If the heap is not empty, attend the event with the earliest end day (i.e., the top of the heap).
4. Each time an event is attended, increment the count.
5. Continue this until all events are considered.

**Time Complexity:** \(O(n \log n)\), due to both sorting and heap operations.

**Space Complexity:** \(O(n)\), mainly for storing events in the heap.

```go
func maxEvents(events [][]int) int {
    // Sort events by their start day
    sort.Slice(events, func(i, j int) bool {
        return events[i][0] < events[j][0]
    })

    minHeap := &MinIntHeap{}
    heap.Init(minHeap)

    i, count, n := 0, 0, len(events)
    // Iterate through all days from the earliest possible start day to the last possible end day
    for day := 1; day <= 100000; day++ {
        // Remove all events from the heap that have ended
        for minHeap.Len() > 0 && (*minHeap)[0] < day {
            heap.Pop(minHeap)
        }

        // Push all events that start today onto the heap
        for i < n && events[i][0] == day {
            heap.Push(minHeap, events[i][1])
            i++
        }

        // If there is an event we can attend today
        if minHeap.Len() > 0 {
            // Attend the event that ends the soonest
            heap.Pop(minHeap)
            count++
        }
    }
    return count
}

// MinIntHeap is a min-heap of ints
type MinIntHeap []int

func (h MinIntHeap) Len() int           { return len(h) }
func (h MinIntHeap) Less(i, j int) bool { return h[i] < h[j] }
func (h MinIntHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *MinIntHeap) Push(x interface{}) {
    *h = append(*h, x.(int))
}

func (h *MinIntHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}
```

---

These solutions offer an increasing level of efficiency, from a simple greedy approach to a more optimal one using a min-heap to ensure that we maximize the number of events attended each day by always choosing the events ending first.

