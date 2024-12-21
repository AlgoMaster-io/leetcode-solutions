## [Minimum Number of Refueling Stops](https://leetcode.com/problems/minimum-number-of-refueling-stops/)

### Approaches
1. [Brute Force (Recursion)](#approach-1)
2. [Dynamic Programming](#approach-2)
3. [Greedy with Max-Heap](#approach-3)

---

### Approach 1: Brute Force (Recursion)

**Intuition:**

The brute force approach explores all possible ways to reach the target by recursively considering each station to see if we should stop there for refueling or not. It's akin to a decision tree where each node represents a choice: to refuel or not at a station.

This approach can be highly inefficient since it checks every possible path, leading to exponential time complexity.

**Detailed Steps:**

1. Create a recursive function with current fuel, current position, and max fuel stops as parameters.
2. If the current position (fuel range) is greater than or equal to the target, return 0 because no more refuels are needed.
3. If the current position exceeds the number of stations, return infinity as it denotes an invalid path.
4. Make recursive calls:
   - Do not refill at the current station and move to the next.
   - Refill at the current station and add one to the count of refuel stops.
5. Return the minimum of both recursive calls.

**Time Complexity:** O(2^n), where n is the number of stations.

**Space Complexity:** O(n) due to recursion stack space.

```go
package main

import "math"

func minRefuelStops(target int, startFuel int, stations [][]int) int {
    return dfs(target, startFuel, stations, 0)
}

func dfs(target, curFuel int, stations [][]int, index int) int {
    // Base case: if we can reach target
    if curFuel >= target {
        return 0
    }
    
    // Base case: if no more station to consider
    if index >= len(stations) || curFuel < stations[index][0] {
        return math.MaxInt32
    }
    
    // No refuel option
    noRefuel := dfs(target, curFuel, stations, index+1)

    // Refuel option
    refuel := dfs(target, curFuel+stations[index][1], stations, index+1)
    
    if refuel != math.MaxInt32 {
        refuel += 1
    }
    
    // Take minimum of both approaches
    return min(noRefuel, refuel)
}

func min(x, y int) int {
    if x < y {
        return x
    }
    return y
}
```

---

### Approach 2: Dynamic Programming

**Intuition:**

To optimize the brute force solution, we can use dynamic programming. We'll maintain a dp array where `dp[i]` represents the farthest distance we can reach with exactly `i` refueling stops.

The solution involves iterating through each station and checking how far we can go using previous stops recorded in `dp`.

**Detailed Steps:**

1. Initialize a dp array of length `n+1` with `dp[0]` as `startFuel` (max distance with 0 stops).
2. For each station, check how it impacts the dp states for previous refueling counts.
3. Update dp values backwards from the possibility of the current index.
4. Return the minimum index `i` where `dp[i]` exceeds or meets the target.

**Time Complexity:** O(n^2), where n is the number of stations.

**Space Complexity:** O(n) for the dp array.

```go
package main

func minRefuelStops(target int, startFuel int, stations [][]int) int {
    n := len(stations)
    dp := make([]int, n+1)
    dp[0] = startFuel
    
    for i := 0; i < n; i++ {
        for j := i; j >= 0; j-- {
            if dp[j] >= stations[i][0] {
                dp[j+1] = max(dp[j+1], dp[j] + stations[i][1])
            }
        }
    }
    
    for i := 0; i <= n; i++ {
        if dp[i] >= target {
            return i
        }
    }
    return -1
}

func max(x, y int) int {
    if x > y {
        return x
    }
    return y
}
```

---

### Approach 3: Greedy with Max-Heap

**Intuition:**

This approach uses a greedy strategy combined with a priority queue (max heap) to always refuel with the largest available fuel when necessary. The idea is to travel as far as possible and only stop to refuel when the tank is empty.

**Detailed Steps:**

1. Use a max-heap priority queue to track fuel capacities.
2. Start from the initial fuel and keep adding possible refuels into the heap as you advance.
3. When the fuel is insufficient to reach further, pop the max fuel from the heap to extend range.
4. Count the number of stops made to reach the target.

**Time Complexity:** O(n log n), where n is the number of stations due to heap operations.

**Space Complexity:** O(n) for the heap.

```go
package main

import (
    "container/heap"
)

type MaxHeap []int

func (h MaxHeap) Len() int            { return len(h) }
func (h MaxHeap) Less(i, j int) bool  { return h[i] > h[j] }
func (h MaxHeap) Swap(i, j int)       { h[i], h[j] = h[j], h[i] }
func (h *MaxHeap) Push(x interface{}) { *h = append(*h, x.(int)) }
func (h *MaxHeap) Pop() interface{}   { old := *h; x := old[len(old)-1]; *h = old[0 : len(old)-1]; return x }

func minRefuelStops(target int, startFuel int, stations [][]int) int {
    fuelStops, pos, fuel := 0, 0, startFuel
    maxHeap := &MaxHeap{}
    heap.Init(maxHeap)
    
    for {
        if fuel >= target {
            return fuelStops
        }
        
        for pos < len(stations) && stations[pos][0] <= fuel {
            heap.Push(maxHeap, stations[pos][1])
            pos++
        }
        
        if maxHeap.Len() == 0 {
            return -1
        }
        
        fuel += heap.Pop(maxHeap).(int)
        fuelStops++
    }
}
```

In the above solutions, we have progressively improved efficiency and performance from a brute force recursive solution to a dynamic programming one, and finally, to a greedy approach using a max-heap to make strategic refueling stops.

