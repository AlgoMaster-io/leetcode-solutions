# [Leetcode 871: Minimum Number of Refueling Stops](https://leetcode.com/problems/minimum-number-of-refueling-stops/)

## Approaches:
1. [Brute Force - Recursion](#approach-1)
2. [Dynamic Programming](#approach-2)
3. [Greedy with Max-Heap](#approach-3)

---

## Approach 1: Brute Force - Recursion

### Intuition
In this approach, we explore all possible paths of stop sequences to reach the target. We recursively decide whether to stop at each station or skip it, which gives us a tree of possibilities. Although straightforward, this method is inefficient due to its exponential time complexity.

```csharp
public class Solution {
    public int MinRefuelStops(int target, int startFuel, int[][] stations) {
        int n = stations.Length;

        // Helper function to perform the recursive exploration
        int FindMinimumStops(int index, int currentFuel) {
            // If current fuel is enough to reach the target
            if (currentFuel >= target) return 0;

            // If all stations are used or cannot reach the next station
            if (index == n || currentFuel < stations[index][0]) return int.MaxValue;

            // Option 1: Skip the station
            int noStop = FindMinimumStops(index + 1, currentFuel);
            
            // Option 2: Refuel at the station
            int withStop = FindMinimumStops(index + 1, currentFuel - stations[index][0] + stations[index][1]);

            if (withStop != int.MaxValue) withStop += 1; // Include this station as a stop

            // Return the minimum of two options
            return Math.Min(noStop, withStop);
        }

        int result = FindMinimumStops(0, startFuel);
        return result == int.MaxValue ? -1 : result;
    }
}
```

### Time and Space Complexity
- **Time Complexity**: O(2^n). Every station has two choices: refuel or not, leading to exponential possibilities.
- **Space Complexity**: O(n). The depth of the recursion tree can be up to the number of stations.

---

## Approach 2: Dynamic Programming

### Intuition
Use dynamic programming to optimize the exploration of possibilities. Define `dp[i]` as the furthest we can reach with `i` refueling stops. We iteratively attempt to use each station, updating the maximum distance that can be reached with varying stops.

```csharp
public class Solution {
    public int MinRefuelStops(int target, int startFuel, int[][] stations) {
        int n = stations.Length;
        long[] dp = new long[n + 1];
        dp[0] = startFuel;

        // Evaluate for each station
        for (int i = 0; i < n; i++) {
            for (int t = i; t >= 0; t--) {
                if (dp[t] >= stations[i][0]) { // Can reach this station
                    dp[t + 1] = Math.Max(dp[t + 1], dp[t] + stations[i][1]);
                }
            }
        }

        // Find the minimum number of stops to reach or exceed the target
        for (int i = 0; i <= n; i++) {
            if (dp[i] >= target) return i;
        }
        return -1;
    }
}
```

### Time and Space Complexity
- **Time Complexity**: O(n^2). For each station, we potentially update every previous stop's possibility.
- **Space Complexity**: O(n). We maintain an array of size `n+1` for possible stops.

---

## Approach 3: Greedy with Max-Heap

### Intuition
Utilizing a priority queue (max-heap) allows us to implement a greedy approach: always refuel with the maximum possible available fuel when necessary. This approach efficiently decides the best station for refueling at each point.

```csharp
using System;
using System.Collections.Generic;

public class Solution {
    public int MinRefuelStops(int target, int startFuel, int[][] stations) {
        PriorityQueue<int, int> maxHeap = new PriorityQueue<int, int>();
        int result = 0, i = 0, n = stations.Length;
        int currentPosition = startFuel;

        while (currentPosition < target) {
            // Add all reachable stations' fuel to the heap
            while (i < n && currentPosition >= stations[i][0]) {
                // Push negative for max-heap behavior
                maxHeap.Enqueue(stations[i][1], -stations[i][1]);
                i++;
            }

            if (maxHeap.Count == 0) return -1; // No reachable station or target

            // Refuel with the largest available fuel from previous stations
            currentPosition += maxHeap.Dequeue();
            result++;
        }

        return result;
    }
}
```

### Time and Space Complexity
- **Time Complexity**: O(n log n). Each station is enqueued and dequeued once.
- **Space Complexity**: O(n). Heap stores up to all station fuels if necessary.

