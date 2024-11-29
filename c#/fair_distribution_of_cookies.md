# 2305. [Fair Distribution of Cookies](https://leetcode.com/problems/fair-distribution-of-cookies/)

## Approach 1: Brute Force - Generate All Distributions

### Solution
```csharp
// Time Complexity: O(k^n)
// Space Complexity: O(k)
public class Solution {
    public int DistributeCookies(int[] cookies, int k) {
        return Helper(cookies, new int[k], 0);
    }

    private int Helper(int[] cookies, int[] distribution, int index) {
        if (index == cookies.Length) {
            int max = int.MinValue;
            foreach (int d in distribution) {
                max = Math.Max(max, d);
            }
            return max;
        }

        int minUnfairness = int.MaxValue;
        for (int i = 0; i < distribution.Length; i++) {
            distribution[i] += cookies[index];
            minUnfairness = Math.Min(minUnfairness, Helper(cookies, distribution, index + 1));
            distribution[i] -= cookies[index];
        }
        return minUnfairness;
    }
}
```

## Approach 2: Backtracking with Pruning

### Solution
```csharp
// Time Complexity: O(k * 2^n)
// Space Complexity: O(k)
public class Solution {
    public int DistributeCookies(int[] cookies, int k) {
        int[] distribution = new int[k];
        return Helper(cookies, distribution, 0, 0, int.MaxValue);
    }

    private int Helper(int[] cookies, int[] distribution, int index, int currentMax, int minUnfairness) {
        if (index == cookies.Length) {
            return Math.Min(minUnfairness, currentMax);
        }

        for (int i = 0; i < distribution.Length; i++) {
            distribution[i] += cookies[index];
            minUnfairness = Helper(cookies, distribution, index + 1, Math.Max(currentMax, distribution[i]), minUnfairness);
            distribution[i] -= cookies[index];

            if (distribution[i] == 0) { // Optimizing to prevent unnecessary states
                break;
            }
        }
        return minUnfairness;
    }
}
```

## Approach 3: Dynamic Programming with Bitmask

### Solution
```csharp
// Time Complexity: O(n*2^n)
// Space Complexity: O(2^n)
public class Solution {
    public int DistributeCookies(int[] cookies, int k) {
        int n = cookies.Length;
        int[] sum = new int[1 << n];
        
        // Precompute the sum of each subset
        for (int i = 0; i < (1 << n); i++) {
            for (int j = 0; j < n; j++) {
                if ((i & (1 << j)) != 0) {
                    sum[i] += cookies[j];
                }
            }
        }

        // dp[mask] is the number of k subsets with the j-th subset (sum of mask is taken) having the minimum maximum sum
        int[][] dp = new int[k + 1][];
        for (int i = 0; i <= k; i++) {
            dp[i] = new int[1 << n];
            for (int j = 0; j < dp[i].Length; j++) {
                dp[i][j] = int.MaxValue;
            }
        }
        
        dp[0][0] = 0; 

        for (int i = 1; i <= k; i++) {
            for (int mask = 0; mask < (1 << n); mask++) {
                for (int submask = mask; submask > 0; submask = (submask - 1) & mask) {
                    dp[i][mask] = Math.Min(dp[i][mask], Math.Max(dp[i - 1][mask ^ submask], sum[submask]));
                }
            }
        }
        
        return dp[k][(1 << n) - 1];
    }
}
```

