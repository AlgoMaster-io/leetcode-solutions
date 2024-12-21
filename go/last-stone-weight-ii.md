# [Leetcode 1049: Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/)

## Approaches
- [Approach 1: Dynamic Programming (Subset Sum problem)](#approach-1-dynamic-programming-subset-sum-problem)


## Approach 1: Dynamic Programming (Subset Sum problem)

### Intuition
The problem of "Last Stone Weight II" can be transformed into a subset sum problem. Our aim is to divide the stones into two groups such that the difference in weight between the two groups is minimized. Therefore, if we can find a subset of stones with a sum that is at most half of the total weight, the difference between the two subset sums will give the least weight for the last stone (or no stone if perfectly balanced).

In simpler terms, consider the minimum achievable value for `|sum(group1) - sum(group2)|`. Our task can be thought of as trying to partition the stones into two groups such that the sum of stones in one of the groups is as close as possible to half of the total sum of all stones.

We use a dynamic programming approach to figure this out. The dp array `dp[i]` will represent whether a subset weight `i` is achievable using these stones.

### Detailed Steps

1. Calculate the total sum of all stones.
2. Determine the target as `target = totalWeight / 2`. Our goal is to find the best sum close to this target.
3. Instantiate a DP array where `dp[i]` will be `true` if a subset with the sum `i` can be formed. Initialize `dp[0] = true`.
4. For each stone, update the dp array from back to front (`target` to `stone`) to avoid overwriting information needed for future updates in the same iteration.
5. The closest sum to `target` that we can achieve is found when `dp[j]` is `true`.
6. The result is finally `totalWeight - 2 * bestAchievable`. This represents the minimum weight of the last stone(s).

### Code

```go
func lastStoneWeightII(stones []int) int {
    totalWeight := 0
    for _, stone := range stones {
        totalWeight += stone
    }

    target := totalWeight / 2
    dp := make([]bool, target+1)
    dp[0] = true

    for _, stone := range stones {
        // Traverse backwards to avoid using the same stone in the same iteration
        for j := target; j >= stone; j-- {
            if dp[j-stone] {
                dp[j] = true
            }
        }
    }

    // Find the closest possible value to target
    for bestAchievable := target; bestAchievable >= 0; bestAchievable-- {
        if dp[bestAchievable] {
            return totalWeight - 2 * bestAchievable
        }
    }

    return 0
}
```

### Time Complexity
- `O(n * sum/2)`, where `n` is the number of stones. This is because we iterate through each stone and for each stone iterate through the possible sums up to `sum/2`.

### Space Complexity
- `O(sum/2)`, which is the size of the dp array to store possible achievable sums.

