# [518. Coin Change II](https://leetcode.com/problems/coin-change-ii/)

## Approaches
1. [Recursive Approach](#recursive-approach)
2. [Top-Down Dynamic Programming with Memoization](#top-down-dynamic-programming-with-memoization)
3. [Bottom-Up Dynamic Programming](#bottom-up-dynamic-programming)
4. [Optimized Space Dynamic Programming](#optimized-space-dynamic-programming)

---

## Recursive Approach

### Intuition
The naive approach involves using recursion to evaluate all possible combinations of coins to make up the amount. We either include a coin in our current combination or skip it, and recursively solve for the remaining amount.

### Code
```cpp
class Solution {
public:
    int coinChange(int amount, vector<int>& coins, int index) {
        // Base case: if we reach a zero amount, we found a combination
        if (amount == 0) return 1;
        // If amount is negative or no more coins are available, no combination is possible
        if (amount < 0 || index >= coins.size()) return 0;
        
        // Recursive calls
        // 1. Include the current coin
        int include = coinChange(amount - coins[index], coins, index);
        // 2. Exclude the current coin and move to the next coin
        int exclude = coinChange(amount, coins, index + 1);
        
        return include + exclude;
    }
    
    int change(int amount, vector<int>& coins) {
        return coinChange(amount, coins, 0);
    }
};
```

### Complexity
- **Time Complexity:** \(O(2^n)\), where \(n\) is the number of coins. This is due to the exponential number of combinations.
- **Space Complexity:** \(O(n)\), due to the recursion stack.

---

## Top-Down Dynamic Programming with Memoization

### Intuition
The recursive solution explores all subproblems, many of which overlap. We can use memoization to store results of already solved subproblems to avoid redundant calculations.

### Code
```cpp
class Solution {
public:
    int dp[301][5001]; // Memoization table, assuming max coins size is 300 and max amount is 5000 based on typical constraints
    
    int coinChange(int amount, vector<int>& coins, int index) {
        if (amount == 0) return 1;
        if (amount < 0 || index >= coins.size()) return 0;
        if (dp[index][amount] != -1) return dp[index][amount];
        
        // Include or exclude current coin
        int include = coinChange(amount - coins[index], coins, index);
        int exclude = coinChange(amount, coins, index + 1);
        
        return dp[index][amount] = include + exclude;
    }
    
    int change(int amount, vector<int>& coins) {
        memset(dp, -1, sizeof(dp));
        return coinChange(amount, coins, 0);
    }
};
```

### Complexity
- **Time Complexity:** \(O(n \times \text{amount})\), where \(n\) is the number of coins.
- **Space Complexity:** \(O(n \times \text{amount})\), due to the memoization table.

---

## Bottom-Up Dynamic Programming

### Intuition
We can iteratively build up the solution starting from smaller subproblems. Use a DP table where dp[i] represents the number of ways to make amount i using available coins.

### Code
```cpp
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        vector<int> dp(amount + 1, 0);
        dp[0] = 1; // Base case
        
        for (int coin : coins) {
            for (int i = coin; i <= amount; ++i) {
                dp[i] += dp[i - coin];
            }
        }
        
        return dp[amount];
    }
};
```

### Complexity
- **Time Complexity:** \(O(n \times \text{amount})\)
- **Space Complexity:** \(O(\text{amount})\)

---

## Optimized Space Dynamic Programming

### Intuition
The previous DP solution already uses \(O(\text{amount})\) space, which is efficient. The optimization here focuses on understanding that the space used is already minimal for this particular problem representation.

### Code
The code remains the same since it's already optimized for space using a single-dimensional array.

### Complexity
- **Time Complexity:** \(O(n \times \text{amount})\)
- **Space Complexity:** \(O(\text{amount})\)

A single-dimensional DP array efficiently computes the number of combinations for each sub-amount by iteratively adding the number of combinations from prior states using current coins.

