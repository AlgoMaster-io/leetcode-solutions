# 322. [Coin Change](https://leetcode.com/problems/coin-change/)

## Approach 1: Recursive Brute Force

### Solution
```cpp
// Time Complexity: O(S^n) where S is the amount and n is the number of coins
// Space Complexity: O(S) for the recursion stack
#include <vector>
#include <climits>
#include <algorithm>

class Solution {
public:
    int coinChange(std::vector<int>& coins, int amount) {
        if (amount == 0) return 0;
        int result = helper(coins, amount);
        return result == INT_MAX ? -1 : result;
    }

private:
    int helper(std::vector<int>& coins, int remainder) {
        if (remainder < 0) return INT_MAX;
        if (remainder == 0) return 0;

        int minCoins = INT_MAX;
        for (int coin : coins) {
            int res = helper(coins, remainder - coin);
            
            // Avoid integer overflow here and take minimum
            if (res != INT_MAX) {
                minCoins = std::min(minCoins, res + 1);
            }
        }
        return minCoins;
    }
};
```

## Approach 2: Dynamic Programming - Bottom Up

### Solution
```cpp
// Time Complexity: O(S * n)
// Space Complexity: O(S)
#include <vector>
#include <algorithm>
#include <cmath>

class Solution {
public:
    int coinChange(std::vector<int>& coins, int amount) {
        std::vector<int> dp(amount + 1, amount + 1);
        dp[0] = 0;

        for (int i = 1; i <= amount; i++) {
            for (int coin : coins) {
                if (i - coin >= 0) {
                    dp[i] = std::min(dp[i], dp[i - coin] + 1);
                }
            }
        }
        
        // If dp[amount] is still amount + 1, no solution found
        return dp[amount] > amount ? -1 : dp[amount];
    }
};
```

## Approach 3: Dynamic Programming - Top Down with Memoization

### Solution
```cpp
// Time Complexity: O(S * n)
// Space Complexity: O(S)
#include <vector>
#include <algorithm>
#include <cstring>

class Solution {
public:
    int coinChange(std::vector<int>& coins, int amount) {
        std::vector<int> memo(amount + 1, -1);
        memo[0] = 0;
        return coinChangeRecursive(coins, amount, memo);
    }

private:
    int coinChangeRecursive(std::vector<int>& coins, int remaining, std::vector<int>& memo) {
        if (remaining < 0) return -1;
        if (memo[remaining] != -1) return memo[remaining];

        int minCoins = INT_MAX;
        for (int coin : coins) {
            int res = coinChangeRecursive(coins, remaining - coin, memo);
            
            if (res >= 0) {
                minCoins = std::min(minCoins, res + 1);
            }
        }

        memo[remaining] = (minCoins == INT_MAX) ? -1 : minCoins;
        return memo[remaining];
    }
};
```

