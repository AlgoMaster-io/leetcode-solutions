[LeetCode 871: Minimum Number of Refueling Stops](https://leetcode.com/problems/minimum-number-of-refueling-stops/)

## Approaches
1. [Greedy Approach with Max Heap](#greedy-approach-with-max-heap)

---

## Greedy Approach with Max Heap

### Intuition:
The problem requires us to determine the minimum number of refueling stops needed to reach a target, given a list of fuel stations. The optimal way is to use a greedy strategy: always refuel at the station with the maximum fuel available that we can reach when needed. This approach lets us prioritize fueling decisions effectively.

### Approach:
- We will use a Max Heap (or a priority queue that supports decreasing order) to keep track of the amounts of fuel available at the stations we've passed.
- We iteratively move from our start to the target, adding stations' fuel amounts to our heap as we reach them.
- When we run out of fuel, we'll refuel with the biggest amount available in our heap (decreasing our total stops by 1 per usage).
- If at any point our target becomes reachable, we return the number of stops made so far.

### Code:
```cpp
#include <vector>
#include <queue>
#include <algorithm>

using namespace std;

int minRefuelStops(int target, int startFuel, vector<vector<int>>& stations) {
    // Max heap to keep track of the fuel amounts in stations we passed
    priority_queue<int> maxHeap;
    
    // Append the target as the last "station" with 0 fuel (makes handling edge case cleaner)
    stations.push_back({target, 0});

    int currentFuel = startFuel;  // This tracks the current fuel
    int stops = 0;                // This tracks the number of stops taken
    int prevPosition = 0;         // The previous station or starting point

    for (const auto& station : stations) {
        int stationPosition = station[0];
        int stationFuel = station[1];
        
        // As long as we don't have enough fuel to reach the current station, refuel at the best previous one
        while (currentFuel < stationPosition - prevPosition) {
            if (maxHeap.empty()) return -1; // There's no fuel to refuel with, so target is unreachable
            currentFuel += maxHeap.top();
            maxHeap.pop();
            stops++;
        }

        // Move to the current station
        currentFuel -= (stationPosition - prevPosition);
        prevPosition = stationPosition;
        maxHeap.push(stationFuel);
    }

    return stops;
}
```

### Detailed Explanation:
- We start at the initial fuel and check each station one by one:
  - If we can't reach the next station, we refuel with the maximum available fuel from the heap.
  - If we can reach the station, we just add its fuel to the heap for future refuel consideration.
- If we exhaust our options in the heap before reaching our target, we return -1 as it means reaching the target isn't feasible.
- This solution efficiently keeps track of necessary refuels using a max heap, ensuring the optimal use of available resources at any step.

### Time and Space Complexity:
- **Time Complexity:** O(n log n). We perform a logarithmic operation to insert and pop elements from the max heap which is n in the worst case scenario of visiting all stations.
- **Space Complexity:** O(n) to store the maximum size of elements in the heap.

