## [Leetcode 1049: Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/)

### Approaches
- [Approach 1: Dynamic Programming Subset Sum](#approach-1-dynamic-programming-subset-sum)

---

### Approach 1: Dynamic Programming Subset Sum

**Intuition:**
The problem is essentially asking us to partition the stones into two groups so that the absolute difference of their weights is minimized. This can be approached like a "partition problem" where we need to find two subsets whose difference is minimized.

If we think in terms of a 0/1 knapsack problem, the target of achieving two subsets with minimum difference is equivalent to trying to fill a knapsack that is half the sum of all stone weights. Our task is to find the closest sum to half the total sum because dividing the stones equally results in the smallest difference.

We can implement this problem using dynamic programming, where our DP array will track which sums are possible with the subsets of stones we've considered so far.

**Algorithm:**
1. Calculate the total sum of all stones.
2. Initialize a DP array of boolean values where `dp[i]` indicates whether a subset with sum `i` can be formed from the elements in the array. Set `dp[0]` to `true` because a sum of zero is always possible without choosing any elements.
3. Iterate over each stone and try to update the dp array from the back (to prevent using the same stone more than once in a subset).
4. For each stone, attempt to update `dp[j]` for `j` from total/2 down to the stone's weight.
5. The answer will be `total - 2 * max possible sum that is `<= total/2``. This ensures the partition is as equal as possible.

**Code:**

```typescript
function lastStoneWeightII(stones: number[]): number {
    const total = stones.reduce((a, b) => a + b, 0);
    const target = Math.floor(total / 2);
    const dp: boolean[] = new Array(target + 1).fill(false);
    dp[0] = true;

    for (const stone of stones) {
        for (let j = target; j >= stone; j--) {
            // If dp[j - stone] is true, it means we can form the sum (j - stone)
            // If we add the stone, we can form the sum j
            dp[j] = dp[j] || dp[j - stone];
        }
    }

    // Find the largest j such that dp[j] is true
    for (let j = target; j >= 0; j--) {
        if (dp[j]) {
            return total - 2 * j; // The minimized difference
        }
    }

    return 0;
}
```

**Time Complexity:** O(n * total/2), where n is the number of stones.

**Space Complexity:** O(total/2), which depends on the computation of target for the dp array.

