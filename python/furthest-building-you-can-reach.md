# [Leetcode 1642: Furthest Building You Can Reach](https://leetcode.com/problems/furthest-building-you-can-reach/)

## Approaches

1. [Brute Force Approach](#brute-force-approach)
2. [Greedy with Max-Heap Approach](#greedy-with-max-heap-approach)

---

## Brute Force Approach

### Intuition
In this approach, we iterate through each building, checking if we can move onto the next building using either bricks or ladders. If the height of the next building is greater than the current one, we need to account for the difference in heights using either a ladder or bricks. 

We'll firstly use ladders whenever possible because they're unlimited in height difference, unlike bricks. If we run out of ladders, then we'll use bricks until we can't anymore. 

This approach, though straightforward and understandable, is not efficient because it doesn't smartly decide when to use ladders, leading to unnecessary exhaustion of resources and a potential inability to go further.

### Code

```python
def furthestBuilding(heights, bricks, ladders):
    for i in range(len(heights) - 1):
        diff = heights[i + 1] - heights[i]
        if diff > 0:  # Need to climb up
            if bricks >= diff:
                bricks -= diff
            elif ladders > 0:
                ladders -= 1
            else:
                return i  # Can't go further
    return len(heights) - 1

# Test case
print(furthestBuilding([4, 2, 7, 6, 9, 14, 12], 5, 1))  # Expected output: 4
```

### Complexity Analysis
- **Time Complexity:** \(O(n)\), where \(n\) is the total number of buildings.
- **Space Complexity:** \(O(1)\), no extra space is needed.

---

## Greedy with Max-Heap Approach

### Intuition
A more optimal way is to utilize a greedy strategy with a max-heap to determine when to use bricks and when to use ladders. The idea is to always use bricks for the smallest jump possible, and keep ladders for larger jumps (which we identify using the max-heap). 

- As we iterate through the heights, we calculate the difference whenever climbing up, pushing these into a max-heap when using bricks.
- If a situation arises where we need more bricks than available, we convert the largest brick usage into a ladder usage (if we have ladders available), essentially "refunding" the bricks used by it.
- This way, we ensure ladders cover the largest jumps, optimizing our ability to reach further.

### Code

```python
import heapq

def furthestBuilding(heights, bricks, ladders):
    max_heap = []  # max heap to store negative values of jumps made with bricks
    for i in range(len(heights) - 1):
        diff = heights[i + 1] - heights[i]
        if diff > 0:
            heapq.heappush(max_heap, -diff)
            bricks -= diff
            if bricks < 0:
                if ladders > 0:
                    bricks += -heapq.heappop(max_heap)  # Convert largest brick usage to a ladder
                    ladders -= 1
                else:
                    return i  # Can't go further without additional resources
    return len(heights) - 1

# Test case
print(furthestBuilding([4, 2, 7, 6, 9, 14, 12], 5, 1))  # Expected output: 4
```

### Complexity Analysis
- **Time Complexity:** \(O(n \log k)\), where \(n\) is the number of buildings and \(k\) is the number of jumps requiring bricks.
- **Space Complexity:** \(O(k)\) for storing the heap of brick usages. 

This solution is optimal in many practical scenarios as it balances the distribution of bricks and ladders efficiently.

