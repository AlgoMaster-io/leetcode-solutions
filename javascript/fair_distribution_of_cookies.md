# 2305. [Fair Distribution of Cookies](https://leetcode.com/problems/fair-distribution-of-cookies/)

## Approach 1: Brute Force - Generate All Distributions

### Solution
javascript
```javascript
// Time Complexity: O(k^n)
// Space Complexity: O(k)
class Solution {
    distributeCookies(cookies, k) {
        return this.helper(cookies, new Array(k).fill(0), 0);
    }

    helper(cookies, distribution, index) {
        if (index === cookies.length) {
            return Math.max(...distribution);
        }

        let minUnfairness = Number.MAX_VALUE;
        for (let i = 0; i < distribution.length; i++) {
            distribution[i] += cookies[index];
            minUnfairness = Math.min(minUnfairness, this.helper(cookies, distribution, index + 1));
            distribution[i] -= cookies[index];
        }
        return minUnfairness;
    }
}
```

## Approach 2: Backtracking with Pruning

### Solution
javascript
```javascript
// Time Complexity: O(k * 2^n)
// Space Complexity: O(k)
class Solution {
    distributeCookies(cookies, k) {
        const distribution = new Array(k).fill(0);
        return this.helper(cookies, distribution, 0, 0, Number.MAX_VALUE);
    }

    helper(cookies, distribution, index, currentMax, minUnfairness) {
        if (index === cookies.length) {
            return Math.min(minUnfairness, currentMax);
        }

        for (let i = 0; i < distribution.length; i++) {
            distribution[i] += cookies[index];
            minUnfairness = this.helper(cookies, distribution, index + 1, Math.max(currentMax, distribution[i]), minUnfairness);
            distribution[i] -= cookies[index];

            if (distribution[i] === 0) { // Optimizing to prevent unnecessary states
                break;
            }
        }
        return minUnfairness;
    }
}
```

## Approach 3: Dynamic Programming with Bitmask

### Solution
javascript
```javascript
// Time Complexity: O(n*2^n)
// Space Complexity: O(2^n)
class Solution {
    distributeCookies(cookies, k) {
        const n = cookies.length;
        const sum = new Array(1 << n).fill(0);
        
        // Precompute the sum of each subset
        for (let i = 0; i < (1 << n); i++) {
            for (let j = 0; j < n; j++) {
                if ((i & (1 << j)) !== 0) {
                    sum[i] += cookies[j];
                }
            }
        }

        // dp[mask] is the number of k subsets with the j-th subset (sum of mask is taken) having the minimum maximum sum
        const dp = Array.from({ length: k + 1 }, () => new Array(1 << n).fill(Number.MAX_VALUE));
        dp[0][0] = 0; 

        for (let i = 1; i <= k; i++) {
            for (let mask = 0; mask < (1 << n); mask++) {
                for (let submask = mask; submask > 0; submask = (submask - 1) & mask) {
                    dp[i][mask] = Math.min(dp[i][mask], Math.max(dp[i - 1][mask ^ submask], sum[submask]));
                }
            }
        }
        
        return dp[k][(1 << n) - 1];
    }
}
```

