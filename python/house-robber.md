# [Leetcode 198: House Robber](https://leetcode.com/problems/house-robber/)

## Approaches
1. [Recursive Brute Force](#recursive-brute-force)
2. [Memoization](#memoization)
3. [Dynamic Programming](#dynamic-programming)
4. [Optimized Dynamic Programming](#optimized-dynamic-programming)

---

### Recursive Brute Force

The simplest approach involves using a recursive function to attempt all combinations of houses to rob. The robber has a choice: rob the current house and then skip to the house after the next one, or skip the current house and consider the next one.

#### Intuition
For each house, you either rob it or skip it. If you rob it, you must skip the next house. Employ recursion to explore each possibility and determine the maximum amount that can be robbed.

```python
def rob_recursive(nums):
    def rob_from(house):
        # Base case: no more houses left to rob
        if house >= len(nums):
            return 0
        # Either rob this house or skip it
        return max(nums[house] + rob_from(house + 2), rob_from(house + 1))
    
    # Start the recursion from the first house
    return rob_from(0)

# Example usage
print(rob_recursive([1, 2, 3, 1]))  # Output: 4
```

**Time Complexity:** O(2^n) because each house creates two recursive calls.

**Space Complexity:** O(n) due to the recursion stack.

---

### Memoization

We can improve upon the brute force by storing results of already computed states to avoid redundant calculations.

#### Intuition
We employ a top-down dynamic programming approach using recursion and memoization. For each house, we store the maximum amount that can be robbed starting from that house in a memoization table, reducing redundant work.

```python
def rob_memoization(nums):
    memo = {}

    def rob_from(house):
        if house >= len(nums):
            return 0
        if house in memo:
            return memo[house]
        # Store result in memo before returning
        memo[house] = max(nums[house] + rob_from(house + 2), rob_from(house + 1))
        return memo[house]

    return rob_from(0)

# Example usage
print(rob_memoization([1, 2, 3, 1]))  # Output: 4
```

**Time Complexity:** O(n) due to solving each subproblem only once.

**Space Complexity:** O(n) for the memoization storage and recursion stack.

---

### Dynamic Programming

We convert our memoization approach into an iterative bottom-up dynamic programming solution.

#### Intuition
Use a DP table where `dp[i]` indicates the maximum amount that can be robbed from house `i` onward. Ultimately, the desired solution is `dp[0]`.

```python
def rob_dp(nums):
    if not nums:
        return 0

    n = len(nums)
    dp = [0] * (n + 1)
    # Base cases
    dp[n] = 0
    dp[n-1] = nums[n-1]

    # Fill the DP table backwards
    for i in range(n - 2, -1, -1):
        dp[i] = max(nums[i] + dp[i + 2], dp[i + 1])

    return dp[0]

# Example usage
print(rob_dp([1, 2, 3, 1]))  # Output: 4
```

**Time Complexity:** O(n) as we solve each subproblem once.

**Space Complexity:** O(n) for the DP table.

---

### Optimized Dynamic Programming

Instead of maintaining a full DP array, we realize that we only need the last two computed values to determine the current one.

#### Intuition
Reduce space usage by maintaining only two variables `prev1` and `prev2` that play the role of `dp[i+1]` and `dp[i+2]`.

```python
def rob_optimized(nums):
    if not nums:
        return 0

    prev1, prev2 = 0, 0
    # Iterate through each house
    for num in nums:
        # Update prev1 and prev2
        temp = max(num + prev2, prev1)
        prev2 = prev1
        prev1 = temp

    return prev1

# Example usage
print(rob_optimized([1, 2, 3, 1]))  # Output: 4
```

**Time Complexity:** O(n) because we iterate through the list once.

**Space Complexity:** O(1) as only two extra variables are used.

