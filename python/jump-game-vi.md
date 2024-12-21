## [LeetCode Problem 1696: Jump Game VI](https://leetcode.com/problems/jump-game-vi/)

### Approaches:
1. [Dynamic Programming with Memoization (Basic Approach)](#dynamic-programming-with-memoization)
2. [Optimized Dynamic Programming with Sliding Window Maximum (Optimal Approach)](#optimized-dp-with-sliding-window-maximum)

### Dynamic Programming with Memoization

**Intuition:**
The idea is to use dynamic programming (DP) where we recursively calculate the maximum score for each position by considering all possible jumps we could make from that position. We use memoization to store the results of subproblems to avoid recalculating them.

For each index `i`, we can jump to any position from `i+1` to `i+k` and we want to maximize the sum of scores from reaching index `i`. We recursively calculate the best score we can achieve starting from each of those positions, adding `nums[i]` to the result.

**Code:**

```python
def maxResult(nums, k):
    n = len(nums)
    memo = [-float('inf')] * n
    
    def dp(i):
        if i == n - 1:
            return nums[i]
        
        if memo[i] != -float('inf'):
            return memo[i]
        
        max_score = -float('inf')
        # Try jumping to each position from i+1 to i+k
        for j in range(i+1, min(i+k+1, n)):
            max_score = max(max_score, dp(j))
        
        memo[i] = nums[i] + max_score
        return memo[i]

    return dp(0)

# Time Complexity: O(n * k) - Recursive approach with memoization
# Space Complexity: O(n) - Memoization array and call stack
```

### Optimized DP with Sliding Window Maximum

**Intuition:**
The dynamic programming approach was inefficient because it recalculated maximum scores for overlapping subproblems in each function call. To optimize this, we can use a sliding window maximum technique where we maintain a deque that allows us to always look up the maximum score from the previous k positions in constant time.

We iterate through the list and, for each position `i`, we use our deque to determine the best previous position to jump from. This way, we maintain a running maximum and update it efficiently.

**Code:**

```python
from collections import deque

def maxResult(nums, k):
    n = len(nums)
    deque_list = deque([0])  # Store indices of `nums`
    for i in range(1, n):
        # Calculate the maximum score to reach index `i`
        # Take the value from the top of the deque
        nums[i] += nums[deque_list[0]]
        
        # Maintain the deque to only have potential max indices
        while deque_list and nums[deque_list[-1]] <= nums[i]:
            deque_list.pop()
        
        deque_list.append(i)
        
        # Remove indices that are out of the window range
        if deque_list[0] == i - k:
            deque_list.popleft()
    
    return nums[-1]

# Time Complexity: O(n) - Each element is pushed to and popped from the deque at most once
# Space Complexity: O(k) - Space for the deque
```

This approach significantly reduces redundant calculations and efficiently keeps track of the maximum score achievable at each step, allowing us to achieve optimal time complexity.

