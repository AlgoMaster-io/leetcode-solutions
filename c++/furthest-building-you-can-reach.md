# [Leetcode 1642: Furthest Building You Can Reach](https://leetcode.com/problems/furthest-building-you-can-reach/)

## Approaches

1. [Brute Force Approach](#brute-force-approach)
2. [Greedy Approach with Priority Queue (Optimal)](#greedy-approach-with-priority-queue-optimal)

---

## Brute Force Approach

### Intuition

Start at the first building and try to go as far as possible using bricks where we're forced to climb up. When unable to progress further with just bricks, you backtrack or stop. However, this approach doesn't incorporate ladders efficiently when determining the farthest distance. It merely serves as a baseline strategy to understand the problem dynamics.

### Steps

1. Traverse through the heights array.
2. Whenever you encounter a height difference (going up), try to use the available bricks.
3. If bricks are insufficient, halt the progress.

### Code

```cpp
int furthestBuilding(vector<int>& heights, int bricks, int ladders) {
    int n = heights.size();
    for (int i = 0; i < n - 1; ++i) {
        int diff = heights[i+1] - heights[i];
        if (diff > 0) {
            if (bricks >= diff) {
                // Use bricks to ascend the building
                bricks -= diff;
            } else {
                // Can't move further as bricks are insufficient
                return i;
            }
        }
    }
    return n - 1;  // Reached the last building
}
```

### Time Complexity

- **Time Complexity**: O(N), where N is the number of buildings.
- **Space Complexity**: O(1).

---

## Greedy Approach with Priority Queue (Optimal)

### Intuition

Utilize priority queues to decide dynamically when to use bricks and when to use ladders by prioritilizing the largest jumps. Here, we leverage a max heap to keep track of the largest jumps made with bricks so that the strategy is reactive to the jumps and allows an optimal decision of replacing a brick-used jump with a ladder, when necessary.

### Steps

1. Initialize a priority queue to store brick-used climbs.
2. Traverse through each building.
3. For every positive height difference (climb required):
   - First, use bricks, and store the brick amount in the priority queue.
   - If bricks run out, pop from the priority queue to simulate using a ladder for the largest climb and rollback bricks, using ladders instead.
4. If ever bricks and ladders both are exhausted, return the current building as the furthest possible.

### Code

```cpp
#include <vector>
#include <queue>
using namespace std;

int furthestBuilding(vector<int>& heights, int bricks, int ladders) {
    priority_queue<int, vector<int>, greater<int>> pq;  // Min-heap for used bricks

    for (int i = 0; i < heights.size() - 1; ++i) {
        int diff = heights[i + 1] - heights[i];
        
        if (diff > 0) {
            pq.push(diff);  // Climb, try using bricks first
            bricks -= diff;
            
            if (bricks < 0) {
                // Not enough bricks, use a ladder for the highest brick-based climb
                bricks += pq.top();  // Add back bricks used for smallest climb
                pq.pop();
                ladders--;

                // If no ladders are left, stop furthest point achievable
                if (ladders < 0) {
                    return i;
                }
            }
        }
    }
    return heights.size() - 1;  // Reached the last building
}
```

### Time Complexity

- **Time Complexity**: O(N log K), where N is the number of buildings and K is the number of climbs (bricks used), as each insertion into the priority queue takes logK time.
- **Space Complexity**: O(K), space for storing the climbs that have been prioritized.

