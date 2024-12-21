# [Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)

## Approaches:
- [Approach 1: Recursive Backtracking](#approach-1-recursive-backtracking)
- [Approach 2: Top-Down Dynamic Programming with Memoization](#approach-2-top-down-dynamic-programming-with-memoization)
- [Approach 3: Dynamic Programming Table (Bottom-Up)](#approach-3-dynamic-programming-table-bottom-up)
- [Approach 4: Optimized Dynamic Programming Using a 1D DP Array](#approach-4-optimized-dynamic-programming-using-a-1d-dp-array)

### Approach 1: Recursive Backtracking

The simplest approach is to use recursive backtracking to try all possible subsets and check if their sum equals half of the total sum.

#### Intuition:
- Calculate the total sum of the array. If it is odd, simply return `False` since it's impossible to split into two equal subsets.
- Use backtracking to explore subsets. For each element, choose to either include it in the current subset or not.
- Keep a running total of the current subset sum and check if it equals half of the total sum.

```python
def canPartition(nums):
    total_sum = sum(nums)
    
    # If the total sum is odd, it cannot be partitioned into two equal subsets
    if total_sum % 2 != 0:
        return False
    
    target = total_sum // 2
    
    def can_find_subset(index, current_sum):
        if current_sum == target:
            return True
        if current_sum > target or index >= len(nums):
            return False
        
        #Choose the current index item or skip it
        return (can_find_subset(index + 1, current_sum + nums[index]) or 
                can_find_subset(index + 1, current_sum))
    
    return can_find_subset(0, 0)
```

**Time Complexity:** Exponential in nature, \(O(2^n)\)  
**Space Complexity:** \(O(n)\) due to recursion stack depth

### Approach 2: Top-Down Dynamic Programming with Memoization

To optimize the recursive approach, we can use memoization to avoid repeated calculations.

#### Intuition:
- Similar to the recursive approach, but use a memo dictionary to store results of subproblems.
- When you encounter a previously calculated subproblem, return the saved result instead of recalculating.

```python
def canPartition(nums):
    total_sum = sum(nums)
    
    if total_sum % 2 != 0:
        return False
    
    target = total_sum // 2
    memo = {}

    def can_find_subset(index, current_sum):
        if current_sum == target:
            return True
        if current_sum > target or index >= len(nums):
            return False
        if (index, current_sum) in memo:
            return memo[(index, current_sum)]
        
        # Store in the memo and return
        memo[(index, current_sum)] = (
            can_find_subset(index + 1, current_sum + nums[index]) or 
            can_find_subset(index + 1, current_sum)
        )
        return memo[(index, current_sum)]
    
    return can_find_subset(0, 0)
```

**Time Complexity:** \(O(n \times (\text{sum}/2))\)  
**Space Complexity:** \(O(n \times (\text{sum}/2))\) due to the memoization dictionary

### Approach 3: Dynamic Programming Table (Bottom-Up)

Use a 2D DP array to iteratively build up a solution.

#### Intuition:
- Use a DP table where `dp[i][j]` is `True` if a subset with sum `j` can be formed using the first `i` numbers.
- Initialize `dp[0][0]` to be `True` (zero sum with zero elements).
- For each number, determine if the current subset sum can be formed.

```python
def canPartition(nums):
    total_sum = sum(nums)
    
    if total_sum % 2 != 0:
        return False
    
    target = total_sum // 2
    n = len(nums)
    dp = [[False] * (target + 1) for _ in range(n + 1)]
    
    for i in range(n + 1):
        dp[i][0] = True  # sum 0 can always be formed with an empty subset

    for i in range(1, n + 1):
        for j in range(1, target + 1):
            dp[i][j] = dp[i - 1][j]
            if j >= nums[i - 1]:
                dp[i][j] |= dp[i - 1][j - nums[i - 1]]

    return dp[n][target]
```

**Time Complexity:** \(O(n \times (\text{sum}/2))\)  
**Space Complexity:** \(O(n \times (\text{sum}/2))\)

### Approach 4: Optimized Dynamic Programming Using a 1D DP Array

By observing that each state in the DP table depends only on the previous row, we can optimize space by using only a single row.

#### Intuition:
- Use a 1D array `dp` where `dp[j]` is `True` if a subset with sum `j` can be formed.
- Iteratively update the array for each element.

```python
def canPartition(nums):
    total_sum = sum(nums)
    
    if total_sum % 2 != 0:
        return False
    
    target = total_sum // 2
    dp = [False] * (target + 1)
    dp[0] = True  # base case: a subset with zero sum is always possible
    
    for num in nums:
        for j in range(target, num - 1, -1):
            dp[j] = dp[j] or dp[j - num]

    return dp[target]
```

**Time Complexity:** \(O(n \times (\text{sum}/2))\)  
**Space Complexity:** \(O(\text{sum}/2)\) due to the 1D array

These approaches progressively optimize the solution from a straightforward exponential recursive solution to an optimized DP solution with linear space complexity.

