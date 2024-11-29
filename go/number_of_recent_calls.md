# 933. [Number of Recent Calls](https://leetcode.com/problems/number-of-recent-calls/)

## Approach 1: Brute Force

### Solution
go
```go
// Time Complexity: O(n) per request (where n is the number of calls in the time window)
// Space Complexity: O(n)
package main

type RecentCounter struct {
    requests []int
}

func Constructor() RecentCounter {
    return RecentCounter{requests: []int{}}
}

func (this *RecentCounter) Ping(t int) int {
    this.requests = append(this.requests, t) // Add the new request
    count := 0

    // Count all requests in the range [t - 3000, t]
    for _, time := range this.requests {
        if time >= t-3000 {
            count++
        }
    }

    return count
}
```

## Approach 2: Using a Queue (Optimal Solution)

### Solution
go
```go
// Time Complexity: O(1) per request (amortized)
// Space Complexity: O(n)
package main

type RecentCounter struct {
    queue []int
}

func Constructor() RecentCounter {
    return RecentCounter{queue: []int{}}
}

func (this *RecentCounter) Ping(t int) int {
    this.queue = append(this.queue, t) // Add the new request to the queue

    // Remove requests that are outside the range [t - 3000, t]
    for len(this.queue) > 0 && this.queue[0] < t-3000 {
        this.queue = this.queue[1:]
    }

    return len(this.queue) // The length of the queue represents the number of valid requests
}
```

