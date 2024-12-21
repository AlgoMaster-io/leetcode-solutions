# [Leetcode 123: Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)

## Approaches

1. [Brute Force Using Recursion](#approach-1)
2. [Dynamic Programming - Top Down Memoization](#approach-2)
3. [Dynamic Programming - Bottom Up](#approach-3)
4. [State Machine Optimization](#approach-4)

---

## Approach 1: Brute Force Using Recursion

### Intuition
The brute force approach tries every possible pair of transactions and keeps track of the maximum profit. This involves making two transactions, each comprising a buy and a sell operation. We would use recursion to explore each possible transaction and then pick the pair that yields the highest profit.

### Time and Space Complexity
- **Time Complexity**: O(n^4) - This is extremely inefficient as we are essentially looking at all pairs of transactions in a recursive manner.
- **Space Complexity**: O(n) - Recursion stack depth.

### Solution

```go
func maxProfit(prices []int) int {
    if len(prices) < 2 {
        return 0
    }
    var maxProfit int
    for i := 0; i < len(prices); i++ {
        maxProfit = max(maxProfit, helper(prices, i))
    }
    return maxProfit
}

func helper(prices []int, start int) int {
    if start >= len(prices) {
        return 0
    }
    maxProfit := 0
    for i := start; i < len(prices); i++ {
        maxProfitWithSale := 0
        for j := i + 1; j < len(prices); j++ {
            if prices[j] > prices[i] {
                maxProfitWithSale = prices[j] - prices[i] + helper(prices, j+1)
            }
            maxProfit = max(maxProfit, maxProfitWithSale)
        }
    }
    return maxProfit
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

---

## Approach 2: Dynamic Programming - Top Down Memoization

### Intuition
This approach aims to optimize the brute force method by storing the results of subproblems. We maintain a 3D memoization table where each entry denotes the maximum profit at a given day with a given transaction in process (buy or sell).

### Time and Space Complexity
- **Time Complexity**: O(n * 3) - For the state (day, transactions, hold).
- **Space Complexity**: O(n * 3) - For the DP table.

### Solution

```go
func maxProfit(prices []int) int {
    dp := make([][][]int, len(prices))
    for i := range dp {
        dp[i] = [][]int{{-1, -1}, {-1, -1}, {-1, -1}}
    }
    return maxProfitFrom(0, 0, 0, prices, dp)
}

func maxProfitFrom(day, transactions, holding int, prices []int, dp [][][]int) int {
    if day >= len(prices) || transactions >= 2 {
        return 0
    }
  
    // Return the stored result to avoid recomputation
    if dp[day][transactions][holding] != -1 {
        return dp[day][transactions][holding]
    }
    
    noAction := maxProfitFrom(day+1, transactions, holding, prices, dp) // Skip to next day
    
    if holding == 1 { // Holding the stock
        // Option 1: Sell the stock
        sell := prices[day] + maxProfitFrom(day+1, transactions+1, 0, prices, dp)
        dp[day][transactions][holding] = max(noAction, sell)
    } else { // Not holding the stock
        // Option 2: Buy the stock
        buy := -prices[day] + maxProfitFrom(day+1, transactions, 1, prices, dp)
        dp[day][transactions][holding] = max(noAction, buy)
    }
    
    return dp[day][transactions][holding]
}
```

---

## Approach 3: Dynamic Programming - Bottom Up

### Intuition
Convert the top-down recursive approach into bottom-up dynamic programming by filling up the DP table in a systematic manner. This ensures each subproblem is computed only once, improving efficiency.

### Time and Space Complexity
- **Time Complexity**: O(n * 3) - Same as the top-down approach.
- **Space Complexity**: O(n * 3) - For the DP table.

### Solution

```go
func maxProfit(prices []int) int {
    if len(prices) == 0 {
        return 0
    }
    n := len(prices)
    dp := make([][][]int, n+1)

    for i := range dp {
        dp[i] = [][]int{{0, 0}, {0, 0}, {0, 0}}
    }
  
    // Fill the DP table
    for day := n - 1; day >= 0; day-- {
        for transactions := 1; transactions >= 0; transactions-- {
            // Holding a stock
            dp[day][transactions][1] = max(dp[day+1][transactions][1], -prices[day]+dp[day+1][transactions][0])
            // Not holding a stock
            dp[day][transactions][0] = max(dp[day+1][transactions][0], prices[day]+dp[day+1][transactions+1][1])
        }
    }
    return dp[0][0][0]
}
```

---

## Approach 4: State Machine Optimization

### Intuition
This is the most optimal method involving a state machine. We use variables to store the maximum profit at different states instead of a DP table. This significantly reduces space usage.

### Time and Space Complexity
- **Time Complexity**: O(n) - We iterate through the price array once.
- **Space Complexity**: O(1) - Constant space usage.

### Solution

```go
func maxProfit(prices []int) int {
    buy1, buy2 := int(^uint(0) >> 1), int(^uint(0) >> 1)
    sell1, sell2 := 0, 0

    for _, price := range prices {
        buy1 = min(buy1, price)
        sell1 = max(sell1, price-buy1)
        buy2 = min(buy2, price-sell1)
        sell2 = max(sell2, price-buy2)
    }
  
    return sell2
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

In this final state machine approach, we effectively track the minimum costs and potential profits as we buy and sell once and then potentially buy and sell again. By keeping variables for the states, we achieve the optimal solution with constant space usage.

