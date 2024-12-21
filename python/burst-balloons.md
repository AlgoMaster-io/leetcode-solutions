# [LeetCode 312: Burst Balloons](https://leetcode.com/problems/burst-balloons/)

## Table of Contents
- [Approach 1: Recursive with Memoization](#approach-1)
- [Approach 2: Dynamic Programming (Bottom-Up)](#approach-2)

---

## Approach 1: Recursive with Memoization

The problem requires us to maximize our coins by **bursting balloons** in an **optimal order**. Here are some key observations that help us solve it recursively:

1. **Choice Independence**: Bursting a balloon \( i \) affects the next and previous balloons. Hence, for each balloon \( i \), coins obtained are `nums[left-1] * nums[i] * nums[right+1]`.

2. **Subproblems**: If the balloon \( i \) is burst last in the subarray, the resulting problem is to find the maximum coins for subarrays `[left, i-1]` and `[i+1, right]`.

3. **Memoization**: To prevent recalculations, use a 2D memoization table where `memo[left][right]` represents the maximum coins obtainable from that subarray.

```python
def maxCoins(nums):
    # Extend nums with virtual boundaries
    nums = [1] + nums + [1]
    n = len(nums)
    
    # Memoization table to store intermediate results
    memo = [[0] * n for _ in range(n)]
    
    def dp(left, right):
        # Base case: No more balloons to burst between left and right
        if left + 1 == right:
            return 0
        
        # Return memoized result if present
        if memo[left][right] > 0:
            return memo[left][right]

        # Initialize max_coins
        max_coins = 0
        
        # Try bursting each balloon i between left and right last
        for i in range(left + 1, right):
            # Coins obtained from bursting balloon i last
            coins = nums[left] * nums[i] * nums[right]
            # Add coins from recursing on subarrays
            coins += dp(left, i) + dp(i, right)
            # Update max_coins
            max_coins = max(max_coins, coins)
            # Debug statement for tracking computation
            # print(f"Bursting ballon {i} last between {left} and {right}, coins: {coins}, max_coins: {max_coins}")

        # Memoize the result
        memo[left][right] = max_coins
        return max_coins
    
    # Start the recursion with the full range
    return dp(0, n - 1)
```

- **Time Complexity**: \(O(n^3)\) due to three nested loops — two for subarray indices and one for choosing the last balloon.
- **Space Complexity**: \(O(n^2)\) for the memoization table.

---

## Approach 2: Dynamic Programming (Bottom-Up)

Instead of relying on recursion, iterate through the possible lengths of subarrays and compute the maximum coins for each subarray using a DP table.

1. **DP Table**: `dp[i][j]` represents the maximum coins obtainable from bursting all balloons between indices \( i \) and \( j \).

2. **Iterative Calculation**: Build the solution iteratively over increasing subarray lengths.

3. **Initialization**: Same idea of extending the `nums` list with virtual balloons.

```python
def maxCoins(nums):
    # Extend nums with virtual boundaries
    nums = [1] + nums + [1]
    n = len(nums)
    
    # DP table to store the maximum coins obtainable from subsequences
    dp = [[0] * n for _ in range(n)]
    
    # Iterate over subarray lengths starting from 2 since length 0 and 1 are trivial
    for length in range(2, n):
        for left in range(n - length):
            right = left + length
            
            # Initialize current maximum coins for subarray
            max_coins = 0
            
            # Try bursting each balloon i between left and right last
            for i in range(left + 1, right):
                # Coins from bursting balloon i last
                coins = nums[left] * nums[i] * nums[right]
                # Add coins from bursting the left and right segments
                coins += dp[left][i] + dp[i][right]
                # Update max_coins
                max_coins = max(max_coins, coins)
                # Debug statement for tracking computation
                # print(f"Bursting balloon {i} last between {left} and {right}, coins: {coins}, max_coins: {max_coins}")
                
            # Store the result in dp table
            dp[left][right] = max_coins
    
    # The result for the full problem is stored here
    return dp[0][n - 1]
```

- **Time Complexity**: \(O(n^3)\) similar to the recursive approach because of three nested loops.
- **Space Complexity**: \(O(n^2)\) for the DP table.

Both approaches effectively solve the problem using dynamic programming principles, with the bottom-up approach often being more intuitive and avoiding potential recursion depth limitations.

