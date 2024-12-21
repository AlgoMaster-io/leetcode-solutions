# [Leetcode 1049: Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/)

## Solutions:
- [Approach 1: Dynamic Programming - Subset Sum Problem](#approach-1)  

---  

### Approach 1: Dynamic Programming - Subset Sum Problem

This problem is a variation of the partition problem, which is a well-known subset sum problem. The goal here is to split the array into two subsets with a minimal sum difference between them.

Intuition:
- Consider the total sum of the stones as `S`.
- We want to partition the stones into two parts `P` and `Q`.
- The smallest possible positive difference between sums of two subsets can be obtained by dividing the array into two subsets with sums as close as possible to `S/2`.
- The idea is to use a dynamic programming (knapsack-like) approach to find the largest subset sum closest to `S/2`.

**Dynamic Programming Approach:**
1. Calculate the total sum `totalSum` of the given stones.
2. Use a dynamic programming array `dp` where `dp[j]` will be true if a subset sum of `j` can be achieved with the stones.
3. Iterate over each stone and, for each stone value, update the `dp` array backwards.
4. Finally, find the largest `j` such that `dp[j]` is true and `j` is less than or equal to `totalSum / 2`. The answer will be `totalSum - 2*j`.

```javascript
function lastStoneWeightII(stones) {
    // Calculate the total sum of the stones
    const totalSum = stones.reduce((acc, stone) => acc + stone, 0);
    // Determine the maximum achievable subset sum (approximately totalSum / 2)
    const target = Math.floor(totalSum / 2);
    // Initialize the dp array where dp[i] informs if a subset sum i is possible
    const dp = new Array(target + 1).fill(false);
    dp[0] = true;
    
    // Iterate over each stone
    for (let stone of stones) {
        // Update dp array for current stone value
        for (let j = target; j >= stone; j--) {
            dp[j] = dp[j] || dp[j - stone];
        }
    }
    
    // Find the largest j for which dp[j] is true
    for (let j = target; j >= 0; j--) {
        if (dp[j]) {
            return totalSum - 2 * j;
        }
    }
    
    return 0;
}
```

**Time Complexity:** `O(n * sum/2)` where `n` is the number of stones and `sum` is the total sum of the stone weights.
**Space Complexity:** `O(sum/2)` for the `dp` array.

This dynamic programming approach efficiently utilizes the subset sum problem idea to partition the stone weights with minimal remaining weight after collisions. By narrowing down the potential sums close to `totalSum / 2`, we minimize computational complexity while still arriving at the correct solution.

