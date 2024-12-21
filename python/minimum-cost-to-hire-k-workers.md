# [Leetcode 857: Minimum Cost to Hire K Workers](https://leetcode.com/problems/minimum-cost-to-hire-k-workers/)

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Approach using Priority Queue](#optimized-approach-using-priority-queue)

---

## Brute Force Approach

### Intuition:
In this problem, our task is to hire exactly `K` workers with minimal cost. Each worker has a "quality" and a "wage". If worker `i` demands at least a specific `wage[i]`, they are willing to work at a certain "ratio", defined as `wage[i] / quality[i]`. The higher the quality, the higher the wage demanded. The primary insight is to examine combinations of workers and compute their costs based on their respective ratios.

### Steps:
1. Consider every possible group of `K` workers from the list.
2. For each group, determine the ratio of the highest demanding worker (as they define the minimum wage that must be met for the entire group).
3. Calculate the total cost for hiring these `K` workers using the highest ratio obtained.
4. Track the minimum cost over all possible combinations of `K`.

This approach can be computationally expensive, given that the combinations of workers might be numerous, thus it's more of a brute force method.

### Complexity:
- **Time Complexity:** `O(N^K)`, where `N` is the number of workers. This arises from evaluating many combinations.
- **Space Complexity:** `O(1)`, as we're primarily tracking minimum cost.

Although direct code is omitted due to its infeasibility for large `N` and `K`, this intuition helps frame a baseline understanding.

---

## Optimized Approach using Priority Queue

### Intuition:
Instead of examining every possible group, consider sorting workers by their "ratio" (wage-to-quality). If we always choose workers with the smallest possible ratio, the associated wage sum will increase at the slowest rate. This implies that the ratio becomes the main cost-driving factor.

### Steps:
1. Create a list of workers, where each worker is represented as a tuple of their ratio and quality.
2. Sort this list based on the ratio.
3. Use a priority queue (max-heap) to manage and quickly replace the largest quality workers in the current group of size K-1.
4. For each worker with the current minimal ratio, calculate the cumulative cost of hiring them along with the top K-1 minimal quality workers maintained in the heap.
5. Continuously track the minimum cost as you iterate through each ratio.

This approach optimizes selection and utilizes a heap data structure for maintaining the quality selection efficiently.

### Code:
```python
import heapq

def mincostToHireWorkers(quality, wage, K):
    # Number of workers
    N = len(quality)
    
    # Create the list of tuples (ratio, quality), where ratio is wage/quality
    workers = sorted([(w / q, q) for w, q in zip(wage, quality)])
    
    # Initialize variables
    min_cost = float('inf')  # minimum cost tracker
    quality_sum = 0  # sum of qualities in current selected group
    max_heap = []  # max-heap to track largest qualities of selected workers
    
    # Iterate over each worker sorted by their ratio
    for ratio, quality in workers:
        # Push the negative quality to create a max-heap since python has a min-heap by default
        heapq.heappush(max_heap, -quality)
        quality_sum += quality
        
        # If heap size exceeds K, remove the worker with the largest quality
        if len(max_heap) > K:
            quality_sum += heapq.heappop(max_heap)
        
        # If heap size is exactly K, calculate the cost
        if len(max_heap) == K:
            min_cost = min(min_cost, quality_sum * ratio)
    
    return min_cost

# Example Usage:
# quality = [10, 20, 5], wage = [70, 50, 30], K = 2
# mincostToHireWorkers(quality, wage, K) -> 105.0
```

### Complexity:
- **Time Complexity:** `O(N log N)`, due to sorting the workers and maintaining the heap size, where `N` is the number of workers.
- **Space Complexity:** `O(N)`, needed for the heap which can grow up to size `K` and storage of workers.

This solution maintains an efficient balance between calculating costs and dynamically sorting through potential workers to hire, achieving optimality in terms of both time and space complexity.

