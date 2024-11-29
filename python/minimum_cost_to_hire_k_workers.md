# 857. [Minimum Cost to Hire K Workers](https://leetcode.com/problems/minimum-cost-to-hire-k-workers/)

## Approach: Sorting and Priority Queue (Max-Heap)

### Solution
```python
# Time Complexity: O(n * log(n) + n * log(k)), where n is the number of workers and k is the number of workers to hire
# Space Complexity: O(k), for the max-heap
from heapq import heappush, heappop

class Solution:
    def mincostToHireWorkers(self, quality, wage, k):
        n = len(quality)
        workers = []

        # Create a list of workers with their wage-to-quality ratio and quality
        for i in range(n):
            workers.append((wage[i] / quality[i], quality[i]))  # Wage-to-quality ratio, quality

        # Sort workers by their wage-to-quality ratio
        workers.sort()

        max_heap = []  # Max-heap for quality (negated to use the min-heap as max-heap)
        total_quality = 0  # Sum of qualities in the current group
        min_cost = float('inf')

        for ratio, q in workers:
            # Push the negative quality to simulate a max-heap
            heappush(max_heap, -q)
            total_quality += q

            # If we exceed k workers, remove the one with the highest quality
            if len(max_heap) > k:
                total_quality += heappop(max_heap)  # Add because it's negated

            # If we have exactly k workers, calculate the cost
            if len(max_heap) == k:
                min_cost = min(min_cost, total_quality * ratio)

        return min_cost
```

