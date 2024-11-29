# 309. [Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

## Approach 1: Recursive with Memoization

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
package main

func maxProfit(prices []int) int {
    memo := make(map[int]int)
    return calculateMaxProfit(prices, 0, memo)
}

func calculateMaxProfit(prices []int, currentDay int, memo map[int]int) int {
    if currentDay >= len(prices) {
        return 0
    }
    if val, ok := memo[currentDay]; ok {
        return val
    }

    maxProfit := 0
    for sellDay := currentDay + 1; sellDay < len(prices); sellDay++ {
        // Calculate the profit for the current transaction
        profit := prices[sellDay] - prices[currentDay] + calculateMaxProfit(prices, sellDay + 2, memo)
        if profit > maxProfit {
            maxProfit = profit
        }
    }

    memo[currentDay] = max(maxProfit, calculateMaxProfit(prices, currentDay + 1, memo))
    return memo[currentDay]
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

## Approach 2: Dynamic Programming with State Variables

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
package main

func maxProfit(prices []int) int {
    if len(prices) == 0 {
        return 0
    }
    
    n := len(prices)
    buy := make([]int, n)
    sell := make([]int, n)
    cooldown := make([]int, n)

    buy[0] = -prices[0]
    sell[0] = 0
    cooldown[0] = 0

    for i := 1; i < n; i++ {
        buy[i] = max(buy[i-1], cooldown[i-1] - prices[i])
        sell[i] = max(sell[i-1], buy[i-1] + prices[i])
        cooldown[i] = max(cooldown[i-1], sell[i-1])
    }

    return max(sell[n-1], cooldown[n-1])
}
```

## Approach 3: Optimized Space Dynamic Programming

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
package main

func maxProfit(prices []int) int {
    if len(prices) == 0 {
        return 0
    }

    buy, sell, cooldown := -prices[0], 0, 0

    for i := 1; i < len(prices); i++ {
        newSell := max(sell, buy + prices[i])
        newBuy := max(buy, cooldown - prices[i])
        cooldown = max(cooldown, sell)
        buy = newBuy
        sell = newSell
    }

    return max(sell, cooldown)
}
```


