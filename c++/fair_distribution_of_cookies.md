# 2305. [Fair Distribution of Cookies](https://leetcode.com/problems/fair-distribution-of-cookies/)

## Approach 1: Brute Force - Generate All Distributions

### Solution
```cpp
// Time Complexity: O(k^n)
// Space Complexity: O(k)
class Solution {
public:
    int distributeCookies(vector<int>& cookies, int k) {
        vector<int> distribution(k, 0);
        return helper(cookies, distribution, 0);
    }

private:
    int helper(vector<int>& cookies, vector<int>& distribution, int index) {
        if (index == cookies.size()) {
            int maxVal = INT_MIN;
            for (int d : distribution) {
                maxVal = max(maxVal, d);
            }
            return maxVal;
        }

        int minUnfairness = INT_MAX;
        for (int i = 0; i < distribution.size(); i++) {
            distribution[i] += cookies[index];
            minUnfairness = min(minUnfairness, helper(cookies, distribution, index + 1));
            distribution[i] -= cookies[index];
        }
        return minUnfairness;
    }
};
```

## Approach 2: Backtracking with Pruning

### Solution
```cpp
// Time Complexity: O(k * 2^n)
// Space Complexity: O(k)
class Solution {
public:
    int distributeCookies(vector<int>& cookies, int k) {
        vector<int> distribution(k, 0);
        return helper(cookies, distribution, 0, 0, INT_MAX);
    }

private:
    int helper(vector<int>& cookies, vector<int>& distribution, int index, int currentMax, int minUnfairness) {
        if (index == cookies.size()) {
            return min(minUnfairness, currentMax);
        }

        for (int i = 0; i < distribution.size(); i++) {
            distribution[i] += cookies[index];
            minUnfairness = helper(cookies, distribution, index + 1, max(currentMax, distribution[i]), minUnfairness);
            distribution[i] -= cookies[index];

            if (distribution[i] == 0) { // Optimizing to prevent unnecessary states
                break;
            }
        }
        return minUnfairness;
    }
};
```

## Approach 3: Dynamic Programming with Bitmask

### Solution
```cpp
// Time Complexity: O(n*2^n)
// Space Complexity: O(2^n)
class Solution {
public:
    int distributeCookies(vector<int>& cookies, int k) {
        int n = cookies.size();
        vector<int> sum(1 << n, 0);

        // Precompute the sum of each subset
        for (int i = 0; i < (1 << n); i++) {
            for (int j = 0; j < n; j++) {
                if ((i & (1 << j)) != 0) {
                    sum[i] += cookies[j];
                }
            }
        }

        // dp[mask] is the number of k subsets with the j-th subset (sum of mask is taken) having the minimum maximum sum
        vector<vector<int>> dp(k + 1, vector<int>(1 << n, INT_MAX));
        dp[0][0] = 0;

        for (int i = 1; i <= k; i++) {
            for (int mask = 0; mask < (1 << n); mask++) {
                for (int submask = mask; submask > 0; submask = (submask - 1) & mask) {
                    dp[i][mask] = min(dp[i][mask], max(dp[i - 1][mask ^ submask], sum[submask]));
                }
            }
        }

        return dp[k][(1 << n) - 1];
    }
};
```

