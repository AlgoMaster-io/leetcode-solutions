# [The Skyline Problem](https://leetcode.com/problems/the-skyline-problem/)

## Approaches:
1. [Basic Priority Queue Approach](#basic-priority-queue-approach)
2. [Optimized Priority Queue with Lazy Removal](#optimized-priority-queue-with-lazy-removal)

### Basic Priority Queue Approach

**Intuition:**

The idea is to use a priority queue to keep track of all the current buildings and maintain their heights in decreasing order. When processing each critical point (start or end of a building), update the priority queue accordingly.

**Steps:**

1. **Extract Events:** For each building, obtain start and end events. Start events increase the height, and end events will decrease the height.
   
2. **Sort Events:** Each event is represented as a coordinate on the x-axis, and we sort them. Priority is given to starting events (since they could start a new peak).

3. **Use Priority Queue:** As we process each event, update the priority queue. For start events, we add the building height, and for end events, we remove the height.

4. **Determine Key Points:** Every time the current maximum height changes, it signifies a new key point in the skyline. Record this by comparing with previously recorded heights.

```python
from heapq import heappush, heappop

def getSkyline(buildings):
    # Extract critical points
    events = []
    for L, R, H in buildings:
        events.append((L, -H, R))
        events.append((R, 0, 0))
    
    # Sort events
    events.sort()
    
    # Resultant list of key points
    result = [[0, 0]]
    
    # Priority queue, containing neg-heights (for max-heap) and points
    live_buildings = [(0, float('inf'))] 
    
    for x, negH, R in events:
        # Remove buildings from pq that are already ended
        while live_buildings[0][1] <= x:
            heappop(live_buildings)
        
        # If it's the start of a building then add it to the queue
        if negH != 0:
            heappush(live_buildings, (negH, R))
        
        # Current max height
        curr_height = -live_buildings[0][0]
        
        # If current max height differs from last recorded key point height
        if result[-1][1] != curr_height:
            result.append([x, curr_height])

    return result[1:]

```

- **Time Complexity:** \(O(N \log N)\), due to sorting events and maintaining a heap.
- **Space Complexity:** \(O(N)\), for the heap and events tracking.

### Optimized Priority Queue with Lazy Removal

**Intuition:**

Improve the above approach by lazy removal of heights from the priority queue using a hashmap to keep track of actual active height count.

**Steps:**

1. **Use Max-Heap:** We are still using a max-heap to store heights with their endpoints. We process start and end points similarly but maintain a count of heights to defer direct removal.

2. **Lazy Removal:** Instead of using `heappop` directly for removal, only remove when we need the top element to operate on.

```python
def getSkyline(buildings):
    from collections import defaultdict
    
    events = []
    for L, R, H in buildings:
        events.append((L, -H))
        events.append((R, H))
    
    events.sort()

    result = [[0, 0]]
    max_heap = [(0, float('inf'))] # dummy for simplifying code logic
    active_buildings = defaultdict(int)  # Maps height to count

    for x, hp in events:
        h = abs(hp)
        if hp < 0:
            # Start of building
            heappush(max_heap, (hp, x))
            active_buildings[h] += 1
        else:
            # End of building
            active_buildings[h] -= 1
        
        # Clean up the heap for any height with 0 effective count
        while max_heap and active_buildings[-max_heap[0][0]] == 0:
            heappop(max_heap)
        
        # Current prominent height
        current_height = -max_heap[0][0]
        if result[-1][1] != current_height:
            result.append([x, current_height])

    return result[1:]

```

- **Time Complexity:** \(O(N \log N)\), due to sorting and heap operations.
- **Space Complexity:** \(O(N)\), for the heap and hashmap to track active heights.

By leveraging data structures effectively, we can improve runtime to handle larger inputs efficiently. By managing our priorities and lazily maintaining the heap, we get performance improvements with minimal additional space.

