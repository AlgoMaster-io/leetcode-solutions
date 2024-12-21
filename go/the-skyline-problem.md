# [Leetcode 218: The Skyline Problem](https://leetcode.com/problems/the-skyline-problem/)

## Approaches
1. [Basic Approach: Brute Force Using Grid](#basic-approach-brute-force-using-grid)
2. [Intermediate Approach: Using Sorted Events and Max-Heap](#intermediate-approach-using-sorted-events-and-max-heap)
3. [Optimal Approach: Sweep Line Algorithm with Priority Queue](#optimal-approach-sweep-line-algorithm-with-priority-queue)

---

### Basic Approach: Brute Force Using Grid

#### Intuition:
The basic brute-force approach uses a grid to determine the height of the skyline at every possible x-coordinate. We iterate over each building and update the height for the range it covers. Finally, we traverse the grid to determine the silhouette.

#### Steps:
- Create an array to represent the grid which covers all x-coordinates from the first to the last building.
- For each building, update the heights in the grid corresponding to the building's start and end points.
- Traverse the grid to form the silhouette by looking for changes in height.

#### Time Complexity:
- **O(N \* W)** where N is the number of buildings and W is the width of the grid (difference between max and min x-coordinate).

#### Space Complexity:
- **O(W)** due to the grid.

```go
func getSkyline(buildings [][]int) [][]int {
    maxW := 0
    for _, building := range buildings {
        if building[1] > maxW {
            maxW = building[1]
        }
    }
    
    heights := make([]int, maxW+1)
    for _, building := range buildings {
        for i := building[0]; i < building[1]; i++ {
            heights[i] = max(heights[i], building[2])
        }
    }
    
    result := [][]int{}
    prevHeight := 0
    for i := 0; i <= maxW; i++ {
        if heights[i] != prevHeight {
            result = append(result, []int{i, heights[i]})
            prevHeight = heights[i]
        }
    }
    
    return result
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

---

### Intermediate Approach: Using Sorted Events and Max-Heap

#### Intuition:
Transform the problem by considering start and end 'events' at each x-coordinate. Maintain a max-heap to determine the current highest building at any x-coordinate.

#### Steps:
- Convert each building into start and end events.
- Sort all events. Start events are processed before end events if they occur at the same x-coordinate.
- Use a max-heap to track current building heights.
- For each event, adjust the max-heap. If a maximum height changes, add a key point.

#### Time Complexity:
- **O(N log N)** due to sorting and heap operations.

#### Space Complexity:
- **O(N)** for storing events and the heap.

```go
import (
    "container/heap"
    "sort"
)

func getSkyline(buildings [][]int) [][]int {
    events := []Event{}
    for _, building := range buildings {
        events = append(events, Event{building[0], -building[2]})
        events = append(events, Event{building[1], building[2]})
    }

    sort.Slice(events, func(i, j int) bool {
        if events[i].x != events[j].x {
            return events[i].x < events[j].x
        }
        return events[i].h < events[j].h
    })

    result := [][]int{}
    maxHeap := &MaxHeap{}
    heap.Init(maxHeap)
    prevMaxHeight := 0
    heap.Push(maxHeap, 0)

    for _, event := range events {
        if event.h < 0 { 
            heap.Push(maxHeap, -event.h)
        } else {
            for i, h := range *maxHeap {
                if h == event.h {
                    heap.Remove(maxHeap, i)
                    break
                }
            }
        }

        currentMaxHeight := (*maxHeap)[0]
        if prevMaxHeight != currentMaxHeight {
            result = append(result, []int{event.x, currentMaxHeight})
            prevMaxHeight = currentMaxHeight
        }
    }

    return result
}

type Event struct {
    x, h int
}

type MaxHeap []int

func (h MaxHeap) Len() int           { return len(h) }
func (h MaxHeap) Less(i, j int) bool { return h[i] > h[j] }
func (h MaxHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *MaxHeap) Push(x interface{}) { *h = append(*h, x.(int)) }

func (h *MaxHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}
```

---

### Optimal Approach: Sweep Line Algorithm with Priority Queue

**This approach is the same as the intermediate as it represents an optimal efficient solution already.**

This approach effectively manages complexity and ensures both time-efficient and space-efficient solutions by leveraging data structures suited for dynamic data, like the priority queue to automatically handle heights as buildings start and end, consequently producing the skyline with efficient computation.

