# 123. [Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)

## Approach 1: Dynamic Programming with State Variables

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
func maxProfit(prices []int) int {
    hold1, hold2 := int(^uint(0) >> 1) * -1, int(^uint(0) >> 1) * -1
    release1, release2 := 0, 0

    for _, price := range prices {
        // First transaction
        release1 = max(release1, hold1 + price) // Sell stock held by hold1
        hold1 = max(hold1, -price)              // Buy stock

        // Second transaction
        release2 = max(release2, hold2 + price) // Sell stock held by hold2
        hold2 = max(hold2, release1 - price)    // Buy stock after first sell
    }

    return release2 // Maximum profit from two transactions
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

## Approach 2: Dynamic Programming with 2D DP Array

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
func maxProfit(prices []int) int {
    if prices == nil || len(prices) <= 1 {
        return 0
    }

    k := 2 // Maximum number of transactions
    n := len(prices)
    // dp[i][j] = maximum profit using at most i transactions by time j
    dp := make([][]int, k+1)
    for i := range dp {
        dp[i] = make([]int, n)
    }

    for i := 1; i <= k; i++ {
        maxDiff := -prices[0]
        for j := 1; j < n; j++ {
            // Previous maximum profit obtained with `i` transactions or profit after selling stock by day `j`
            dp[i][j] = max(dp[i][j-1], prices[j]+maxDiff)
            // Max profit after buying stock on day `j`
            maxDiff = max(maxDiff, dp[i-1][j]-prices[j])
        }
    }

    return dp[k][n-1] // Maximum profit with at most `k` transactions at the last day
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

## Approach 3: Forward and Backward Pass

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
func maxProfit(prices []int) int {
    n := len(prices)
    if n <= 1 {
        return 0
    }

    leftProfits := make([]int, n)
    rightProfits := make([]int, n)

    minPrice := prices[0]
    for i := 1; i < n; i++ {
        minPrice = min(minPrice, prices[i])
        leftProfits[i] = max(leftProfits[i-1], prices[i]-minPrice)
    }

    maxPrice := prices[n-1]
    for i := n - 2; i >= 0; i-- {
        maxPrice = max(maxPrice, prices[i])
        rightProfits[i] = max(rightProfits[i+1], maxPrice-prices[i])
    }

    maxProfit := 0
    for i := 0; i < n; i++ {
        maxProfit = max(maxProfit, leftProfits[i]+rightProfits[i])
    }

    return maxProfit
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

