## [Leetcode 871: Minimum Number of Refueling Stops](https://leetcode.com/problems/minimum-number-of-refueling-stops/)

### Approaches:
1. [Dynamic Programming Approach](#dynamic-programming-approach)
2. [Greedy Priority Queue Approach](#greedy-priority-queue-approach)

---

### Dynamic Programming Approach

The first approach involves using dynamic programming. The core idea is to maintain a dp array where `dp[i]` represents the maximum distance that can be reached with `i` refueling stops. The key insight is that if we are able to get to a station, then using this station allows us to update the `dp` values with potential new maximum reachable distances.

#### Intuition:

- We initialize a dp array of size N + 1 (where N is the number of stations) with all elements as zero, except `dp[0] = startFuel` since with 0 stops, we can only travel the distance given by the initial fuel.
- Iterate over each station and for each station, update the dp array from the back (to avoid using the same station multiple times within an iteration).
- For each `i`, update `dp[i]` to be the maximum between its current value and `dp[i - 1] + fuel from the station`, provided we could reach the current station (`dp[i - 1] >= required distance to current station`).

#### Time Complexity:
- O(n^2) where n is the number of stations, due to iterating over each station and updating the DP array.

#### Space Complexity:
- O(n) due to the dp array.

```typescript
function minRefuelStops(target: number, startFuel: number, stations: number[][]): number {
    const n = stations.length;
    const dp: number[] = Array(n + 1).fill(0);
    dp[0] = startFuel;
    
    for (let i = 0; i < n; ++i) {
        for (let t = i; t >= 0; --t) {
            // If previous stop is reachable
            if (dp[t] >= stations[i][0]) {
                // Update the dp array for the current number of stops
                dp[t + 1] = Math.max(dp[t + 1], dp[t] + stations[i][1]);
            }
        }
    }
    
    for (let i = 0; i <= n; ++i) {
        // Find the minimum stops needed to reach target
        if (dp[i] >= target) return i;
    }
    
    return -1;
}
```

---

### Greedy Priority Queue Approach

Instead of keeping track of all reachable distances for each possible stop count, a greedy approach uses a maximum heap (priority queue) to keep track of the reachable fuel stations. Always choose the station that provides the most fuel at the current moment when the fuel runs out.

#### Intuition:

- Start from the initial fuel and iterate through the distance, adding any fuel stations that are reachable into a max heap.
- Whenever we run out of fuel to reach the next station, refuel with the largest fuel amounts available from previous stations.
- This solution works in a greedy manner because it always chooses the largest fuel options first, minimizing the number of stops needed.

#### Time Complexity:
- O(n log n) as each station may involve both an insertion and extraction from the heap.

#### Space Complexity:
- O(n) due to the heap storage of stations.

```typescript
function minRefuelStops(target: number, startFuel: number, stations: number[][]): number {
    const maxHeap: number[] = [];
    let currentFuel = startFuel;
    let stops = 0;
    let idx = 0;
    const n = stations.length;

    while (currentFuel < target) {
        // Push all reachable stations fuel into the heap
        while (idx < n && stations[idx][0] <= currentFuel) {
            maxHeap.push(stations[idx][1]);
            maxHeap.sort((a, b) => b - a); // Maintain max heap property
            idx++;
        }
        if (maxHeap.length === 0) {
            return -1; // Cannot reach the target
        }
        // Refuel with the largest capacity
        currentFuel += maxHeap.shift()!;
        stops++;
    }
    
    return stops;
}
```

Both approaches offer a detailed method to solve the problem of minimizing the number of refueling stops. Depending on the constraints, one may choose between these two based on the required performance considerations.

