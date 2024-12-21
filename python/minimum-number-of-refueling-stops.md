# [Leetcode 871: Minimum Number of Refueling Stops](https://leetcode.com/problems/minimum-number-of-refueling-stops/)

## Navigation
- [Approach 1: Dynamic Programming](#dynamic-programming)
- [Approach 2: Greedy with Max-Heap](#greedy-with-max-heap)

## Approach 1: Dynamic Programming

### Intuition
The idea is to use a dynamic programming approach to track how far we can get with a certain number of refueling stops. We can define an array `dp` where `dp[i]` represents the maximum distance we can reach with `i` refueling stops. Initially, with zero refueling, the only distance we can reach is `startFuel`. For each station, we iterate backwards over `dp` to update it: specifically, we calculate if stopping at this station (adding fuel to our tank) extends the distance we can reach with an additional refuel.

### Step-by-Step Code

```python
def minRefuelStops(target, startFuel, stations):
    n = len(stations)
    # dp[i] means the farthest location we can get with i refuels
    dp = [0] * (n + 1)
    dp[0] = startFuel
    
    for i in range(n):
        # Iterate backwards to avoid overwriting dp[i]
        for t in range(i, -1, -1):
            # Check if we can reach stations[i] with t refuels
            if dp[t] >= stations[i][0]:
                # Update dp[t+1] using the fuel from stations[i]
                dp[t+1] = max(dp[t+1], dp[t] + stations[i][1])
    
    for i in range(n+1):
        if dp[i] >= target:
            return i
    return -1
```

### Complexity
- **Time Complexity**: \(O(n^2)\) - We fill a table of size \(n+1\) and update it in a nested loop over stations.
- **Space Complexity**: \(O(n)\) - We use an array `dp` of size \(n+1\).

## Approach 2: Greedy with Max-Heap

### Intuition
A more efficient approach leverages a max-heap (or priority queue) to always refuel from the most amount of fuel available among past stations, which is optimal since those fuels might not be needed later. We maintain a max-heap of all the fuels we have passed but not yet used. When the fuel of our vehicle is about to run out, we use the largest stored fuel. This strategy minimizes the number of stops by always making use of the best resource available.

### Step-by-Step Code

```python
import heapq

def minRefuelStops(target, startFuel, stations):
    max_heap = []
    stations.append((target, 0))  # Add target as a station with 0 fuel
    
    curr_pos, fuel, num_stops = 0, startFuel, 0
    
    for location, refuel_amount in stations:
        fuel -= (location - curr_pos)
        while max_heap and fuel < 0:
            # Use fuel from the furthest/distanced station (maximum fuel in heap)
            fuel += -heapq.heappop(max_heap)
            num_stops += 1
        
        if fuel < 0:
            return -1
        
        heapq.heappush(max_heap, -refuel_amount)
        curr_pos = location
    
    return num_stops
```

### Complexity
- **Time Complexity**: \(O(n \log n)\) - Each operation on the heap takes \(O(\log n)\), and we might need to push/pop each station (in the worst case).
- **Space Complexity**: \(O(n)\) - Storing at most all station fuel amounts in the heap.

This problem can show significant improvement by converting from dynamic programming to a heap-based greedy strategy, especially when dealing with a large number of stations.

