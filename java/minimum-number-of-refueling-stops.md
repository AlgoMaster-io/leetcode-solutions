## [Leetcode 871: Minimum Number of Refueling Stops](https://leetcode.com/problems/minimum-number-of-refueling-stops/)

### Approaches:
1. [Brute Force Recursive Approach](#Brute-Force-Recursive-Approach)
2. [Dynamic Programming Approach](#Dynamic-Programming-Approach)
3. [Greedy Approach with Max-Heap](#Greedy-Approach-with-Max-Heap)

### Brute Force Recursive Approach

**Intuition:**

The brute-force approach involves trying all possible combinations of using the available fuel stops to reach the target. We can implement a recursive function that explores each possible stopping point and decides whether to refuel there or not.

While a possible solution, this approach is highly inefficient and not feasible for large input sizes due to its exponential time complexity.

**Implementation:**

```java
public class MinRefuelStopsBruteForce {
    
    public int minRefuelStops(int target, int startFuel, int[][] stations) {
        int result = refuel(target, startFuel, stations, 0, 0);
        return result == Integer.MAX_VALUE ? -1 : result;
    }

    private int refuel(int target, int currentFuel, int[][] stations, int currentPosition, int numOfStops) {
        // Base case: If currentFuel is already enough to reach the target
        if(currentFuel >= target) return numOfStops;

        // If we have exhausted all stations and cannot reach the target
        if(currentPosition >= stations.length) return Integer.MAX_VALUE;
        
        // Option 1: Do not refuel at the current station
        int skipCurrent = refuel(target, currentFuel, stations, currentPosition + 1, numOfStops);

        int refuelCurrent = Integer.MAX_VALUE;
        
        if(currentFuel >= stations[currentPosition][0]) {
            // Option 2: Refuel at current station
            refuelCurrent = refuel(
                target, 
                currentFuel + stations[currentPosition][1], 
                stations, 
                currentPosition + 1, 
                numOfStops + 1
            );
        }
        
        return Math.min(skipCurrent, refuelCurrent);
    }
}
```

**Time Complexity:**

- Exponential due to exploring all combinations: \(O(2^N)\), where \(N\) is the number of stations.

**Space Complexity:**

- Depth of recursion: \(O(N)\).

### Dynamic Programming Approach

**Intuition:**

We can reduce the time complexity by using dynamic programming. Create an array `dp` where `dp[i]` is the maximum distance we can reach with `i` refueling stops. Our goal is to find the smallest `i` such that `dp[i] >= target`.

**Implementation:**

```java
public class MinRefuelStopsDP {
    
    public int minRefuelStops(int target, int startFuel, int[][] stations) {
        int n = stations.length;
        long[] dp = new long[n + 1];
        dp[0] = startFuel;

        for (int i = 0; i < n; i++) {
            // Traverse backwards to prevent using this station more than once
            for (int t = i; t >= 0; t--) {
                if (dp[t] >= stations[i][0]) {
                    dp[t + 1] = Math.max(dp[t + 1], dp[t] + stations[i][1]);
                }
            }
        }

        for (int i = 0; i <= n; i++) {
            if (dp[i] >= target) {
                return i;
            }
        }
        
        return -1;
    }
}
```

**Time Complexity:**

- \(O(N^2)\), iterating over stations and stops.

**Space Complexity:**

- \(O(N)\) for the dp array.

### Greedy Approach with Max-Heap

**Intuition:**

The most optimal solution involves using a max-heap to prioritize the largest fuel available at each decision point. This approach picks the station with the most fuel capacity reachable based on the current fuel status at each step.

**Implementation:**

```java
import java.util.PriorityQueue;

public class MinRefuelStopsGreedy {

    public int minRefuelStops(int target, int startFuel, int[][] stations) {
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a);
        int fuel = startFuel;
        int stops = 0;
        int i = 0;

        while (fuel < target) {
            // Add all reachable stations to the heap
            while (i < stations.length && stations[i][0] <= fuel) {
                maxHeap.offer(stations[i++][1]);
            }
            
            // If no more stations can be reached and target not achieved
            if (maxHeap.isEmpty()) return -1;
            
            // Refuel with the station having maximum gas
            fuel += maxHeap.poll();
            stops++;
        }
        return stops;
    }
}
```

**Time Complexity:**

- \(O(N \log N)\), primarily due to heap operations.

**Space Complexity:**

- \(O(N)\) for the heap structure.

Each approach progressively optimizes the solution while maintaining correctness, moving from simple brute force to efficient greedy usage of resources available.

