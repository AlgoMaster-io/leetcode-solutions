## [Leetcode 123: Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)

### Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Dynamic Programming (3D DP)](#approach-2-dynamic-programming-3d-dp)
- [Approach 3: Optimized Dynamic Programming](#approach-3-optimized-dynamic-programming)

### Approach 1: Brute Force

#### Intuition
The brute force solution involves trying all possible pairs of transactions that can be made on different days. For each transaction, try buying and selling on every possible pair of days. Calculate all these combinations and choose the one with the maximum profit.

#### Code
```csharp
public int MaxProfit(int[] prices) {
    if (prices == null || prices.Length == 0) return 0;
    int n = prices.Length, maxProfit = 0;

    for (int i = 0; i < n; i++) {
        int maxProfitFirst = 0, maxProfitSecond = 0;

        // Finding the maximum possible profit from the first transaction
        for (int j = 0; j < i; j++) {
            maxProfitFirst = Math.Max(maxProfitFirst, prices[i] - prices[j]);
        }

        // Finding the maximum possible profit from the second transaction
        for (int j = i + 1; j < n; j++) {
            for (int k = i + 1; k < j; k++) {
                maxProfitSecond = Math.Max(maxProfitSecond, prices[j] - prices[k]);
            }
        }

        maxProfit = Math.Max(maxProfit, maxProfitFirst + maxProfitSecond);
    }

    return maxProfit;
}
```

#### Time Complexity
- O(n^3), where n is the number of days since we're iterating over combinations of transactions.

#### Space Complexity
- O(1), as we are not using any extra space.

### Approach 2: Dynamic Programming (3D DP)

#### Intuition
We can use dynamic programming to keep track of the maximum profits. We'll define `dp[i][k][0]` as the maximum profit on day `i` with `k` transactions completed and no stock in hand, and `dp[i][k][1]` as the maximum profit with a stock in hand.

Given that you can complete at most 2 transactions, the approach leverages the DP table to calculate maximum profits by transitioning from one state to another only once for both buy/sell by maximum transaction allowed.

#### Code
```csharp
public int MaxProfit(int[] prices) {
    if (prices == null || prices.Length == 0) return 0;
    int n = prices.Length;
    int[,,] dp = new int[n, 3, 2];  // Transition matrix for the days, transactions and stock status

    for (int i = 0; i < n; i++) {
        for (int k = 2; k > 0; k--) {
            if (i == 0) {
                dp[i, k, 0] = 0;
                dp[i, k, 1] = -prices[i];
            } else {
                dp[i, k, 0] = Math.Max(dp[i - 1, k, 0], dp[i - 1, k, 1] + prices[i]); // Max of not having stock / selling
                dp[i, k, 1] = Math.Max(dp[i - 1, k, 1], dp[i - 1, k - 1, 0] - prices[i]); // Max of having stock / buying
            }
        }
    }

    return dp[n - 1, 2, 0];  // Max profit with 2 transactions on the last day
}
```

#### Time Complexity
- O(n*K), where n is the number of days and K is the max transactions (2). This simplifies to O(n).

#### Space Complexity
- O(n*K), storing the profit states, where K is always 3 (transactions 0, 1, or 2).

### Approach 3: Optimized Dynamic Programming

#### Intuition
We can optimize the space consumption by noticing that the results of day `i` only depend on day `i-1`. Therefore, we can reduce our three-dimensional DP array into a one-dimensional array by reusing previous calculations.

#### Code
```csharp
public int MaxProfit(int[] prices) {
    if (prices == null || prices.Length == 0) return 0;
    int n = prices.Length;
    
    int[] dp_k_0 = new int[3];  // dp array for 0 stock
    int[] dp_k_1 = new int[3];  // dp array for 1 stock, starting -infinity like base price can't be 0

    dp_k_1[0] = dp_k_1[1] = dp_k_1[2] = int.MinValue;

    foreach (int price in prices) {
        for (int k = 2; k > 0; k--) {
            dp_k_0[k] = Math.Max(dp_k_0[k], dp_k_1[k] + price);  // Max profit without stock
            dp_k_1[k] = Math.Max(dp_k_1[k], dp_k_0[k - 1] - price);  // Max profit with stock
        }
    }

    return dp_k_0[2];
}
```

#### Time Complexity
- O(n), iterating over the prices for calculation.

#### Space Complexity
- O(1), optimizing the space usage by using only necessary variables to track states.

