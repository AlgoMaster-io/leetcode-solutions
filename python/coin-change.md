# [Leetcode 322: Coin Change](https://leetcode.com/problems/coin-change/)

## Approaches
1. [Brute Force: Recursion](#approach-1-brute-force-recursion)
2. [Dynamic Programming: Bottom-Up](#approach-2-dynamic-programming-bottom-up)
3. [Dynamic Programming: Top-Down (Memoization)](#approach-3-dynamic-programming-top-down-memoization)

## Approach 1: Brute Force: Recursion

### Intuition:
The basic idea is to try every possible combination of coins to form the amount. For each coin, we have two choices:
- Take the coin and reduce the problem size.
- Skip the coin.

### Steps:
1. If the amount becomes zero, return 0 as no further coins are needed.
2. If the amount becomes negative, return a large number to indicate this is not a valid solution.
3. For each coin, recursively compute the minimum number of coins needed for the remaining amount (i.e., amount - coin).
4. Among all coins, take the minimum of the results for valid solutions.
5. The base case will handle when a valid combination is found, and a recursive case will handle choosing from recursion.

### Complexity:
- **Time Complexity:** \(O(S^n)\), where \(S\) is the amount and \(n\) is the number of coins. This is because for each state(amount), we explore every coin.
- **Space Complexity:** \(O(S)\) due to the recursion depth.

```python
def coinChange(coins, amount):
    # Helper function for recursion
    def dfs(remaining):
        # Base case: when the amount is 0, no coins are needed
        if remaining == 0:
            return 0
        # Return a large number if remaining is negative (invalid path)
        if remaining < 0:
            return float('inf')
        
        # Initialize minimum number of coins needed
        min_coins = float('inf')
        for coin in coins:
            # Recursive case: explore deeper with reduced amount
            res = dfs(remaining - coin)
            # If res is valid, update min_coins
            if res != float('inf'):
                min_coins = min(min_coins, 1 + res)
        
        return min_coins
    
    result = dfs(amount)
    # If result is still float('inf'), it means no valid combination was found
    return result if result != float('inf') else -1
```

## Approach 2: Dynamic Programming: Bottom-Up

### Intuition:
We can improve the recursive solution by using a bottom-up dynamic programming approach. The idea here is to build a DP array where `dp[i]` represents the minimum number of coins required to form the amount `i`.

### Steps:
1. Initialize a DP array where `dp[0] = 0` because zero coins are needed to form the amount zero.
2. For each amount from 1 to `amount`, determine the minimum number of coins needed by trying each coin.
3. If a coin can be used (i.e., `amount - coin >= 0`), update `dp[amount]` as the minimum of current `dp[amount]` and `dp[amount - coin] + 1`.
4. The answer will be stored in `dp[amount]`. If `dp[amount]` is still a large number, return -1 as it indicates it's not possible to form the amount.

### Complexity:
- **Time Complexity:** \(O(n \times S)\), where \(n\) is the number of coins and \(S\) is the amount.
- **Space Complexity:** \(O(S)\) for the DP array.

```python
def coinChange(coins, amount):
    # Initialize dp array with inf for all sums except zero
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0
    
    # Fill the dp array for each amount from 1 to amount
    for i in range(1, amount + 1):
        for coin in coins:
            if i - coin >= 0:
                # Update dp[i] with the minimum coins required
                dp[i] = min(dp[i], dp[i - coin] + 1)
    
    # If dp[amount] is inf, it means we cannot form the amount with given coins
    return dp[amount] if dp[amount] != float('inf') else -1
```

## Approach 3: Dynamic Programming: Top-Down (Memoization)

### Intuition:
This approach combines the ideas of recursion and dynamic programming. By storing the results of subproblems (memoization), we can avoid re-computing the minimum coins for the same amount multiple times.

### Steps:
1. Use a memo dictionary to store the result of each amount.
2. Define a recursive function similar to the brute force approach, but with memoization.
3. For each amount, if it's already in the memo, return the stored result.
4. Otherwise, compute the result recursively and store it in the memo.
5. Finally, return the result for the required amount.

### Complexity:
- **Time Complexity:** \(O(n \times S)\), where \(n\) is the number of coins and \(S\) is the amount.
- **Space Complexity:** \(O(S)\) due to the recursion depth and memoization dictionary.

```python
def coinChange(coins, amount):
    # Initialize memo for storing results of subproblems
    memo = {}
    
    def dfs(remaining):
        # Base case: zero remaining amount needs zero coins
        if remaining == 0:
            return 0
        # Return a large number if remaining is negative (invalid path)
        if remaining < 0:
            return float('inf')
        # Return the stored result to avoid recomputation
        if remaining in memo:
            return memo[remaining]
        
        # Initialize minimum coins as infinity
        min_coins = float('inf')
        for coin in coins:
            # Recursive case: reduce problem size by using current coin
            result = dfs(remaining - coin)
            if result != float('inf'):
                min_coins = min(min_coins, result + 1)
        
        # Store the computed result in memo
        memo[remaining] = min_coins
        return min_coins
    
    result = dfs(amount)
    # If still infinity, no solution was possible
    return result if result != float('inf') else -1
```

Each approach builds on the last, optimizing time complexity by avoiding unnecessary recalculations and efficiently handling subproblems.

