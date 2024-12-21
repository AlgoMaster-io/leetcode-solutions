# [Leetcode 494: Target Sum](https://leetcode.com/problems/target-sum/)

## Approaches
1. [Backtracking Approach](#backtracking-approach)
2. [Dynamic Programming (Memoization) Approach](#dynamic-programming-memoization-approach)
3. [Dynamic Programming (Tabulation) Approach](#dynamic-programming-tabulation-approach)
4. [Optimized Dynamic Programming with Space Efficiency](#optimized-dynamic-programming-with-space-efficiency)

---

## Backtracking Approach

### Intuition
The simplest approach involves using a backtracking technique. In this approach, we will recursively explore all possible combinations of putting `+` and `-` signs in front of each integer in the array. By calculating each possible sum, we can check if it equals the target sum. This method works well for small values of n but becomes inefficient as n grows due to the exponential nature of combinations.

### Code
```python
def findTargetSumWays(nums, target):
    def backtrack(index, current_sum):
        # Base case: if we've processed all numbers
        if index == len(nums):
            # Check if the current_sum equals target
            return 1 if current_sum == target else 0
        
        # Compute the number of ways by adding and subtracting nums[index]
        add = backtrack(index + 1, current_sum + nums[index])
        subtract = backtrack(index + 1, current_sum - nums[index])
        
        # Return total ways adding and subtracting
        return add + subtract

    # Start backtracking from index 0 with initial sum 0
    return backtrack(0, 0)
```

### Complexity
- **Time Complexity**: O(2^n), where n is the number of elements in `nums`. Each number can be independently added or subtracted.
- **Space Complexity**: O(n) for the recursion stack.

---

## Dynamic Programming (Memoization) Approach

### Intuition
To avoid recomputation in the backtracking approach, we can use memoization. By storing the results of subproblems, we save on redundant calculations. We'll store the number of ways to reach a particular sum with a given index.

### Code
```python
def findTargetSumWays(nums, target):
    from collections import defaultdict
    memo = defaultdict(int)

    def backtrack(index, current_sum):
        # If the pair (index, current_sum) is already computed, return the result from memo
        if (index, current_sum) in memo:
            return memo[(index, current_sum)]
        
        # Base case: if we reach the end of nums, check if current_sum is target
        if index == len(nums):
            return 1 if current_sum == target else 0
        
        # Calculate the number of ways to add and subtract current element
        add = backtrack(index + 1, current_sum + nums[index])
        subtract = backtrack(index + 1, current_sum - nums[index])
        
        # Save the result to memoization table
        memo[(index, current_sum)] = add + subtract
        
        return memo[(index, current_sum)]

    return backtrack(0, 0)
```

### Complexity
- **Time Complexity**: O(n * target_sum), where `target_sum` is the range of possible sums. This is due to memoization caching solved subproblems.
- **Space Complexity**: O(n * target_sum) for the memoization table storing subproblem solutions.

---

## Dynamic Programming (Tabulation) Approach

### Intuition
Instead of using recursion, we can fill a DP table iteratively. The table will store the count of ways to obtain each possible sum at every index, translating our recursive logic into an iterative bottom-up dynamic programming implementation.

### Code
```python
def findTargetSumWays(nums, target):
    from collections import defaultdict
    
    total_sum = sum(nums)
    if abs(target) > total_sum: 
        return 0
    dp = [defaultdict(int) for _ in range(len(nums) + 1)]
    dp[0][0] = 1  # Base case: one way to sum up to 0 with 0 elements
    
    for i, num in enumerate(nums, 1):
        for s in range(-total_sum, total_sum + 1):
            if s - num in dp[i - 1]:
                dp[i][s] += dp[i - 1][s - num]
            if s + num in dp[i - 1]:
                dp[i][s] += dp[i - 1][s + num]
    
    return dp[len(nums)][target]
```

### Complexity
- **Time Complexity**: O(n * total_sum), where `total_sum` is the range of possible sums (from `-total_sum` to `total_sum`).
- **Space Complexity**: O(n * total_sum) for the DP table storing possible sums at each step.

---

## Optimized Dynamic Programming with Space Efficiency

### Intuition
We can optimize the space complexity a bit further by operationally maintaining only the current and previous row of DP table since we only depend on the previous row to compute the current row in our dynamic programming approach.

### Code
```python
def findTargetSumWays(nums, target):
    from collections import defaultdict
    
    total_sum = sum(nums)
    if abs(target) > total_sum: 
        return 0
    current = defaultdict(int)
    current[0] = 1

    for num in nums:
        next_dp = defaultdict(int)
        for s in current:
            next_dp[s + num] += current[s]
            next_dp[s - num] += current[s]
        current = next_dp
    
    return current[target]
```

### Complexity
- **Time Complexity**: O(n * total_sum), same as the iterative DP approach.
- **Space Complexity**: O(total_sum) as we only maintain two rows of the DP table at a time.

This solution set progresses from simpler yet inefficient methods to more optimal and efficient ones, striking a balance between conceptual simplicity and computational efficiency.

