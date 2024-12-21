# [Leetcode 518: Coin Change II](https://leetcode.com/problems/coin-change-ii/)

## Approaches
- [Approach 1: Recursion with Memoization](#approach-1-recursion-with-memoization)
- [Approach 2: Dynamic Programming (Bottom-Up)](#approach-2-dynamic-programming-bottom-up)
- [Approach 3: Dynamic Programming with Space Optimization](#approach-3-dynamic-programming-with-space-optimization)

---

## Approach 1: Recursion with Memoization

### Intuition
This problem is a classic example of the Knapsack problem. We can start by defining a recursive solution that considers each coin and counts the number of ways to make up the total amount using that coin or not. Then, we use memoization to store results of subproblems to optimize performance and avoid redundant calculations.

### Solution

```python
def change(amount: int, coins: list[int]) -> int:
    # Create a memoization dictionary
    memo = {}

    def dp(i, remaining):
        # Base case: if remaining amount is zero, there's exactly one way to make change: use no coins
        if remaining == 0:
            return 1
        # If we've gone through all coins or the remaining amount is negative, no solution exists
        if i == len(coins) or remaining < 0:
            return 0
        # If this state has already been computed, return the cached value
        if (i, remaining) in memo:
            return memo[(i, remaining)]

        # Recursive count of including the coin at index i and not including it
        num_ways = dp(i, remaining - coins[i]) + dp(i + 1, remaining)
        # Save the result in the memo dictionary
        memo[(i, remaining)] = num_ways

        return num_ways

    return dp(0, amount)
```

### Time Complexity
- The time complexity is `O(n * amount)` where `n` is the number of coins, due to each subproblem being computed once due to memoization.

### Space Complexity
- The space complexity is also `O(n * amount)`, which accounts for the recursion stack and the memo dictionary storing results for subproblems.

---

## Approach 2: Dynamic Programming (Bottom-Up)

### Intuition
The DP solution uses a table where `dp[i][j]` represents the number of ways to form amount `j` using the first `i` types of coins. The idea is to build this table iteratively using previous results.

### Solution

```python
def change(amount: int, coins: list[int]) -> int:
    # Initialize DP table with base case
    dp = [[0] * (amount + 1) for _ in range(len(coins) + 1)]
    for i in range(len(coins) + 1):
        dp[i][0] = 1  # One way to make amount 0 (use no coins)

    # Fill the DP table
    for i in range(1, len(coins) + 1):
        for j in range(1, amount + 1):
            # Default to solution without including this coin
            dp[i][j] = dp[i - 1][j]
            # Include this coin, if possible
            if j >= coins[i - 1]:
                dp[i][j] += dp[i][j - coins[i - 1]]

    # The answer is the number of ways to make change for the full amount using all coins
    return dp[len(coins)][amount]
```

### Time Complexity
- The time complexity is `O(n * amount)` as we are filling a table with dimensions `n x amount`.

### Space Complexity
- The space complexity is `O(n * amount)` for the table storing intermediate results.

---

## Approach 3: Dynamic Programming with Space Optimization

### Intuition
The above DP table can be reduced to a single-dimensional array as each calculation only depends on the current row and the previous element of the same row. Thus, we can optimize space by using a single array that updates in place.

### Solution

```python
def change(amount: int, coins: list[int]) -> int:
    # Initialize a 1D DP array with base case
    dp = [0] * (amount + 1)
    dp[0] = 1  # Base case: Only one way to create amount 0

    # Iterate over the coins
    for coin in coins:
        for j in range(coin, amount + 1):
            # Update DP array in place
            dp[j] += dp[j - coin]

    # The answer for using all coins for the given amount
    return dp[amount]
```

### Time Complexity
- The time complexity remains `O(n * amount)` as we traverse through coins and target amounts.

### Space Complexity
- The space complexity is `O(amount)`, reducing from `O(n * amount)` in the previous 2D DP approach.

