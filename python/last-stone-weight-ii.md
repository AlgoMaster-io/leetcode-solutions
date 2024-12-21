# [Leetcode 1049: Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/)

## Approaches:
- [Dynamic Programming - Subset Sum](#dynamic-programming---subset-sum)

---

### Dynamic Programming - Subset Sum

#### Intuition:
The problem can be translated into a subset sum problem. We are asked to partition the stones into two groups such that the absolute difference of their weights is minimized.

- Calculate the total weight sum of all stones.
- The goal is to find a subset of these stones whose sum is as close as possible to half of the total weight sum.
- If one of the subset sums is `S1`, then the sum of the other subset is `S2 = total - S1`.
- We need to minimize `|S1 - S2| = |total - 2 * S1|`.

By finding a subset whose sum is the largest possible but not exceeding `total//2`, we can minimize the expression.

#### Solution:

```python
def lastStoneWeightII(stones):
    # Calculate the total sum of stones
    total = sum(stones)
    # The target is to get as close to half of total as possible
    target = total // 2
    # Creating a DP array to track subset sums
    dp = [False] * (target + 1)
    dp[0] = True  # Base case: The sum of zero is always possible
    
    # Iterate through each stone
    for stone in stones:
        for j in range(target, stone - 1, -1):
            if dp[j - stone]:
                dp[j] = True
    
    # Find the largest value of j which is True
    for i in range(target, -1, -1):
        if dp[i]:
            # The resulting minimal weight difference will be total - 2 * i
            return total - 2 * i

# Example Usage
# stones = [2,7,4,1,8,1]
# The output will be 1
```

#### Detailed Explanation and Comments:
- We start by calculating the `total` which is the sum of all stones.
- The target is half of the `total` because we want to minimize `|total - 2 * S1|`.
- We use a boolean array `dp` to keep track of all possible subset sums that can be made with the given stones.
- `dp[j]` will be `True` if a subset sum of `j` is possible.
- We initialize `dp[0]` to `True` since a sum of `0` is achievable with an empty set.
- For each stone, we iterate backward to update `dp` to make sure we don't use the same stone more than once.
- Lastly, we look for the largest `i` where `dp[i]` is `True` - this `i` gives the closest possible sum to half the `total`.

#### Time Complexity:
- `O(n * s)`, where `n` is the number of stones and `s` is the target (which is `total//2`). We iterate through all the stones for all possible sums up to `total // 2`.

#### Space Complexity:
- `O(s)`, where `s` is the target value. We maintain an array of size `target + 1`.

