# [Coin Change](https://leetcode.com/problems/coin-change/)

## Solutions

- [Brute Force Recursive Approach](#brute-force-recursive-approach)
- [Optimized Recursive Memoization](#optimized-recursive-memoization)
- [Bottom-Up Dynamic Programming](#bottom-up-dynamic-programming)

## Brute Force Recursive Approach

### Intuition:

The problem is to find the minimum number of coins needed to make up a given amount. A brute force solution can be achieved by trying every possible combination of coins to make the amount and selecting the one with the fewest number of coins.

The recursive function will try to subtract each coin from the amount and recurse until the amount becomes zero, returning the number of coins used. If the amount becomes negative, we return a large number to indicate that this path isn't feasible.

### Code:

```cpp
#include <iostream>
#include <vector>
#include <climits>
using namespace std;

class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        return recursive(coins, amount);
    }
    
    int recursive(vector<int>& coins, int amount) {
        // Base condition, if amount is zero
        if (amount == 0) return 0;

        // If amount becomes negative, return a large number indicating an invalid path
        if (amount < 0) return INT_MAX;

        int minCoins = INT_MAX;
        for (int coin : coins) {
            int result = recursive(coins, amount - coin);
            if (result != INT_MAX) {
                minCoins = min(minCoins, result + 1);
            }
        }
        
        return minCoins;
    }
};

int main() {
    Solution sol;
    vector<int> coins = {1, 2, 5};
    int amount = 11;
    cout << sol.coinChange(coins, amount) << endl; // Output: 3
    return 0;
}
```

### Time Complexity:

- Exponential — `O(S^n)`, where `S` is the amount, and `n` is the number of different coins. This results from the recursion tree branching `n` times until the base case on each level.

### Space Complexity:

- Call stack can use space up to `O(S)` due to recursion.

## Optimized Recursive Memoization

### Intuition:

To improve the above solution, we use memoization to store results of subproblems, avoiding redundant calculations for the same subproblem. This ensures that each amount is computed only once.

### Code:

```cpp
#include <iostream>
#include <vector>
#include <climits>
using namespace std;

class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        vector<int> memo(amount + 1, -1);
        return recursive(coins, amount, memo);
    }
    
    int recursive(vector<int>& coins, int amount, vector<int>& memo) {
        if (amount == 0) return 0;
        if (amount < 0) return INT_MAX;
        if (memo[amount] != -1) return memo[amount];

        int minCoins = INT_MAX;
        for (int coin : coins) {
            int result = recursive(coins, amount - coin, memo);
            if (result != INT_MAX) {
                minCoins = min(minCoins, result + 1);
            }
        }
        
        memo[amount] = minCoins;
        return minCoins;
    }
};

int main() {
    Solution sol;
    vector<int> coins = {1, 2, 5};
    int amount = 11;
    cout << sol.coinChange(coins, amount) << endl; // Output: 3
    return 0;
}
```

### Time Complexity:

- `O(S*n)`, where `S` is the amount, and `n` is the number of coins. We solve each subproblem once.

### Space Complexity:

- `O(S)` for the recursion call stack and additional space for memoization.

## Bottom-Up Dynamic Programming

### Intuition:

This approach uses dynamic programming to build up the solution from smaller subproblems. We maintain a `dp` array where `dp[i]` represents the minimum number of coins needed for amount `i`. Initialize `dp[0]` to 0 since no coins are needed to make an amount of 0. Then, for each coin and each sub-amount, update the `dp` array accordingly.

### Code:

```cpp
#include <iostream>
#include <vector>
#include <climits>
using namespace std;

class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        vector<int> dp(amount + 1, amount + 1);
        dp[0] = 0;

        for (int i = 1; i <= amount; ++i) {
            for (int coin : coins) {
                if (i - coin >= 0) {
                    dp[i] = min(dp[i], dp[i - coin] + 1);
                }
            }
        }
        return dp[amount] > amount ? -1 : dp[amount];
    }
};

int main() {
    Solution sol;
    vector<int> coins = {1, 2, 5};
    int amount = 11;
    cout << sol.coinChange(coins, amount) << endl; // Output: 3
    return 0;
}
```

### Time Complexity:

- `O(S*n)`, where `S` is the amount and `n` is the number of coins.

### Space Complexity:

- `O(S)` for the `dp` array.

