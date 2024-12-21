## [Leetcode 1049: Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/)

### Approaches:
1. [Dynamic Programming - Subset Sum](#dynamic-programming-subset-sum)

### Dynamic Programming - Subset Sum

To solve this problem, we'll use a dynamic programming approach similar to the Knapsack or Subset Sum problem. The intuition is to partition the stones into two groups such that the difference between their total weights is minimized, which corresponds to achieving the minimum possible last stone weight.

#### Intuition:

1. **Problem Restatement**: We aim to partition the given stones' weights into two subsets, say S1 and S2, so that the absolute difference `|S1 - S2|` is minimized. If we consider `S1` to be the sum of stones in one pile and `S2` to be the sum of stones in the other pile, then we are interested in minimizing `|S1 - S2|`.

2. **Total Sum Calculation**: Let the total weight of stones be `W`. If we can partition the stones into subsets with sums `S1` and `S2`, then `S1 + S2 = W`.

3. **Target Subsum**: Our goal becomes finding a subset with sum as close to `W/2` as possible. If `S1` is close to `W/2`, then `S2 = W - S1` will also be close to `W/2`, minimizing the difference.

4. **Dynamic Programming Approach**: We use a DP array where `dp[j]` will be `true` if a sum `j` can be made with the available stones.

5. **Transition**: For each stone, we update possible sums in the DP array. If we can make sum `j - weight` with the first `i-1` stones, we can also make sum `j` with the first `i` stones.

6. **Result**: The best result will be the maximum sum `S1` that we can make such that `S1 <= W/2`. The answer is `max(W - 2 * S1)` over all valid `S1`.

#### Code Implementation:

```cpp
class Solution {
public:
    int lastStoneWeightII(vector<int>& stones) {
        int totalSum = 0;
        for (int stone : stones) {
            totalSum += stone; // Calculate the total sum of all stones
        }
        
        int target = totalSum / 2;
        vector<bool> dp(target + 1, false);
        dp[0] = true; // There's always a way to make sum 0, by choosing no stone
        
        for (int stone : stones) {
            // Traverse backwards to avoid overwriting results needed for this iteration
            for (int j = target; j >= stone; --j) {
                dp[j] = dp[j] || dp[j - stone]; // Check if the current stone can form the sum 
            }
        }
        
        for (int s1 = target; s1 >= 0; --s1) {
            if (dp[s1]) { // Find the largest subset sum less than or equal to totalSum/2
                return totalSum - 2 * s1; // The minimized last stone weight
            }
        }
        
        return 0; // Default case (though unreachable due to input constraints)
    }
};
```

#### Complexity Analysis:

- **Time Complexity**: \(O(N \times \text{sum})\), where \(N\) is the number of stones and \(\text{sum}\) is the total weight of all stones divided by 2. Each stone is considered against possible sums up to \(\text{target}\).

- **Space Complexity**: \(O(\text{sum})\), which is the size of the DP array. This space is effectively reduced compared to considering all possible sums.

This dynamic programming solution efficiently balances and partitions weights to achieve the smallest possible last stone weight outcome.

