# [Leetcode 933: Number of Recent Calls](https://leetcode.com/problems/number-of-recent-calls/)

## Approaches
- [Approach 1: Naive List-Based Approach](#approach-1-naive-list-based-approach)
- [Approach 2: Optimized Approach Using Queue (Sliding Window Technique)](#approach-2-optimized-approach-using-queue-sliding-window-technique)

## Approach 1: Naive List-Based Approach

### Intuition
The naive solution stores all `ping` timestamps in a list and then iterates through this list to count how many of them are within the last 3000 milliseconds. This is easy to implement, but as the number of pings increases, this becomes inefficient due to repeatedly scanning the entire list of timestamps with each new ping.

### Implementation

```go
type RecentCounter struct {
    pings []int
}

func Constructor() RecentCounter {
    return RecentCounter{pings: []int{}}
}

func (this *RecentCounter) Ping(t int) int {
    this.pings = append(this.pings, t) // Store the incoming timestamp
    count := 0
    for _, ping := range this.pings {
        if ping >= t-3000 {
            count++
        }
    }
    return count
}
```

### Complexity Analysis
- **Time Complexity:** O(n), where n is the number of pings stored. Each `Ping` operation requires a full scan through the list.
- **Space Complexity:** O(n), as we store each timestamp.

## Approach 2: Optimized Approach Using Queue (Sliding Window Technique)

### Intuition
Instead of storing all pings, use a sliding window (queue) that only keeps pings within the last 3000 milliseconds. As each `ping` comes in, remove time stamps that are outside this window. This approach provides an efficient solution by ensuring at any given time, we are only storing relevant timestamps.

### Implementation

```go
type RecentCounter struct {
    pings []int
}

func Constructor() RecentCounter {
    return RecentCounter{pings: []int{}}
}

func (this *RecentCounter) Ping(t int) int {
    this.pings = append(this.pings, t) // Add the new ping timestamp
    
    // Pop out elements from the front if they are older than t-3000
    for len(this.pings) > 0 && this.pings[0] < t-3000 {
        this.pings = this.pings[1:] // remove the oldest element from the queue
    }
    return len(this.pings) // number of elements in the queue is the count of recent pings
}
```

### Complexity Analysis
- **Time Complexity:** O(1) average time for each `Ping` call, because each element is enqueued and dequeued at most once.
- **Space Complexity:** O(w), where w is the maximum number of pings within any 3000ms window, as we only store the relevant pings.

