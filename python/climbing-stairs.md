# [Leetcode 70: Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

## Approaches
1. [Recursive Approach](#recursive-approach)
2. [Memoization Approach](#memoization-approach)
3. [Dynamic Programming Approach (Tabulation)](#dynamic-programming-approach-tabulation)
4. [Optimized Dynamic Programming with Constant Space](#optimized-dynamic-programming-with-constant-space)


### Recursive Approach

**Intuition**:
The problem can be broken down recursively. If you are on a given step `i`, you can either come from step `i-1` or step `i-2`. This is a classic example of a problem that can be solved using recursion. However, since this method recalculates results for the same inputs, it is inefficient for large `n`.

```python
def climbStairs(n: int) -> int:
    # Base case: if there are 0 or 1 steps, there is only one way to climb.
    if n <= 1:
        return 1
    
    # Recursive call considering the two options (i-1) and (i-2)
    return climbStairs(n-1) + climbStairs(n-2)

# Example usage:
# print(climbStairs(5))  # Output: 8
```

**Complexity Analysis**:
- **Time Complexity**: \(O(2^n)\) because each step can branch into two recursive calls.
- **Space Complexity**: \(O(n)\) due to the recursive call stack.


### Memoization Approach

**Intuition**:
To optimize the recursive method, we use a memoization technique to store results of subproblems so they are not recalculated every time.

```python
def climbStairs(n: int, memo=None) -> int:
    if memo is None:
        memo = {}
    
    # Base case where 0 or 1 steps only have one possible way.
    if n <= 1:
        return 1
    
    # If the result is already in the memo dictionary, return it.
    if n in memo:
        return memo[n]
    
    # Calculate and memoize the result.
    memo[n] = climbStairs(n-1, memo) + climbStairs(n-2, memo)
    return memo[n]

# Example usage:
# print(climbStairs(5))  # Output: 8
```

**Complexity Analysis**:
- **Time Complexity**: \(O(n)\) due to storing and utilizing previously computed results.
- **Space Complexity**: \(O(n)\) due to the storage used by the memo dictionary and the recursive call stack.


### Dynamic Programming Approach (Tabulation)

**Intuition**:
Instead of solving the problem top-down with memoization, we can solve it bottom-up using a DP array where each entry represents the number of ways to reach that step. This eliminates the recursive overhead.

```python
def climbStairs(n: int) -> int:
    if n <= 1:
        return 1
    
    dp = [0] * (n + 1)
    dp[0], dp[1] = 1, 1

    # Filling dp array iteratively
    for i in range(2, n + 1):
        dp[i] = dp[i - 1] + dp[i - 2]
    
    return dp[n]

# Example usage:
# print(climbStairs(5))  # Output: 8
```

**Complexity Analysis**:
- **Time Complexity**: \(O(n)\) because we compute the result for each step once.
- **Space Complexity**: \(O(n)\) due to the dp array storage.


### Optimized Dynamic Programming with Constant Space

**Intuition**:
The above solution can be further optimized by noting that we only ever need to know the last two calculated values, which means we can achieve the desired result with constant space by maintaining two variables.

```python
def climbStairs(n: int) -> int:
    if n <= 1:
        return 1
    
    # Initialize the first two values as given in the approach.
    first, second = 1, 1

    # Iteratively update the first and second to simulate the dp array with constant space.
    for i in range(2, n + 1):
        first, second = second, first + second  # Second is the current step count

    return second

# Example usage:
# print(climbStairs(5))  # Output: 8
```

**Complexity Analysis**:
- **Time Complexity**: \(O(n)\) since we only loop through the required calculations once.
- **Space Complexity**: \(O(1)\) because it uses a fixed amount of extra space regardless of `n`.

