## [Leetcode Problem 918: Maximum Sum Circular Subarray](https://leetcode.com/problems/maximum-sum-circular-subarray/)

This problem can be approached using multiple strategies. Below, I have detailed these approaches from basic to more optimal solutions: 

### Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Kadane’s Algorithm Modification](#approach-2-kadanes-algorithm-modification)
- [Approach 3: Optimized Kadane’s with Circular Consideration](#approach-3-optimized-kadanes-with-circular-consideration)

### Approach 1: Brute Force

**Intuition**

The brute force method involves calculating the sum of all possible subarrays, both non-circular and circular. In terms of a circular array, we can think of concatenating the array to itself and then calculating possible subarrays that span the seam between the former end and new beginning.

**Code**

```python
def maxSubarraySumCircular(A):
    n = len(A)
    max_sum = float('-inf')
    
    for i in range(n):
        current_sum = 0
        
        for j in range(n):
            current_sum += A[(i+j) % n]
            max_sum = max(max_sum, current_sum)
    
    return max_sum
```

**Time Complexity:** O(n^2), where n is the length of the array. We calculate the sum for subarrays starting at each element.

**Space Complexity:** O(1). We use constant additional space.

### Approach 2: Kadane’s Algorithm Modification

**Intuition**

Kadane's algorithm is a popular method for finding the maximum sum subarray in a linear array. To adapt it to the circular case, a clever insight is required. We calculate two things: the maximum subarray in the linear sense using Kadane’s algorithm and the maximum subarray sum wrapping around by utilizing the idea of prefix and suffix sums.

**Code**

```python
def maxSubarraySumCircular(A):
    def kadane(gen):
        # Regular Kadane's algorithm for max subarray sum
        best = curr = float('-inf')
        for x in gen:
            curr = x + max(curr, 0)
            best = max(best, curr)
        return best
    
    S = sum(A)
    max_kadane = kadane(A)
    max_wrap = S + kadane(-A[i] for i in range(1, len(A)))
    max_wrap = max(max_wrap, S + kadane(-A[i] for i in range(len(A)-1)))
    
    return max(max_kadane, max_wrap)
```

**Time Complexity:** O(n). Kadane’s algorithm runs in linear time, and we run it a few times with different setups.

**Space Complexity:** O(1). Only constant space is used apart from the input.

### Approach 3: Optimized Kadane’s with Circular Consideration

**Intuition**

In this most optimal approach, we reconsider the relationship between the maximum subarray and minimum subarray. We employ Kadane’s Algorithm to find both the maximum and minimum subarray sums. The idea is that, using the total sum of the list, `max_wrap` can alternatively be obtained using the total array sum subtracting the minimum subarray sum. We need to ensure that the result of `max_wrap` is meaningful and not a full negation of the array if the minimum subarray is the entire array.

**Code**

```python
def maxSubarraySumCircular(A):
    total = sum(A)
    
    def kadane(g):
        curr = ans = g[0]
        for x in g[1:]:
            curr = x + max(curr, 0)
            ans = max(ans, curr)
        return ans
    
    max_kadane = kadane(A)
    min_kadane = kadane([-x for x in A])
    
    # If min_kadane is total, it means all values are negative
    max_wrap = total + min_kadane if total != -min_kadane else max_kadane
    
    return max(max_kadane, max_wrap)
```

**Time Complexity:** O(n). Each function call for Kadane's runs in linear time.

**Space Complexity:** O(1). Constants for calculation despite augmenting the input during execution. 

This provides us with an optimal solution that works efficiently even for large input sizes while addressing the circular nature of the array correctly.

