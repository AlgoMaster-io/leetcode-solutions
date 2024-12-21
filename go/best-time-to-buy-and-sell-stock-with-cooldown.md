[Leetcode Problem 309: Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

Approaches:
- [Naive Recursive Solution](#naive-recursive-solution)
- [Recursive Solution with Memoization](#recursive-solution-with-memoization)
- [Iterative Dynamic Programming](#iterative-dynamic-programming)

### Naive Recursive Solution

#### Intuition
In the naive recursion approach, we attempt to calculate the maximum profit we can achieve by making decisions at each day: to buy, sell, or cooldown. The decision tree is explored fully here, where at each index, we decide whether to buy, sell, or skip. The cooldown condition implies that we can only buy again after a previous sell if we wait one day.

The logic is as follows:
- If we buy, we move to the next index with a negative transaction.
- If we sell, we must sell the stock and then move to index + 2 because of the cooldown.
- If we do nothing, we simply move to the next index.

#### Code
```go
func maxProfit(prices []int) int {
    return maxProfitRec(prices, 0, false)
}

func maxProfitRec(prices []int, currentIndex int, hasStock bool) int {
    if currentIndex >= len(prices) {
        return 0
    }
    
    if hasStock {
        // Two choices: sell today or cooldown without selling
        sell := prices[currentIndex] + maxProfitRec(prices, currentIndex + 2, false)
        cooldown := maxProfitRec(prices, currentIndex + 1, true)
        return max(sell, cooldown)
    } else {
        // Two choices: buy today or cooldown without buying
        buy := -prices[currentIndex] + maxProfitRec(prices, currentIndex + 1, true)
        cooldown := maxProfitRec(prices, currentIndex + 1, false)
        return max(buy, cooldown)
    }
}

func max(x, y int) int {
    if x > y {
        return x
    }
    return y
}
```

#### Complexity
- **Time Complexity**: O(2^n) where n is the number of days, as each day has 2 options and explores two paths recursively.
- **Space Complexity**: O(n) due to the recursion stack.

### Recursive Solution with Memoization

#### Intuition
Memoization is applied to the basic recursive approach to save overlapping sub-problem results. By storing the results of completed computations (i.e., maxProfit at each index and state), we can avoid recalculating them, thus reducing the recursion tree into a more manageable form.

#### Code
```go
func maxProfit(prices []int) int {
    memo := make(map[[2]int]int)
    return maxProfitRecMemo(prices, 0, false, memo)
}

func maxProfitRecMemo(prices []int, currentIndex int, hasStock bool, memo map[[2]int]int) int {
    if currentIndex >= len(prices) {
        return 0
    }
    
    state := [2]int{currentIndex, btoi(hasStock)}
    if val, found := memo[state]; found {
        return val
    }
    
    var result int
    if hasStock {
        sell := prices[currentIndex] + maxProfitRecMemo(prices, currentIndex + 2, false, memo)
        cooldown := maxProfitRecMemo(prices, currentIndex + 1, true, memo)
        result = max(sell, cooldown)
    } else {
        buy := -prices[currentIndex] + maxProfitRecMemo(prices, currentIndex + 1, true, memo)
        cooldown := maxProfitRecMemo(prices, currentIndex + 1, false, memo)
        result = max(buy, cooldown)
    }
    
    memo[state] = result
    return result
}

func btoi(b bool) int {
    if b {
        return 1
    }
    return 0
}
```

#### Complexity
- **Time Complexity**: O(n) since we compute each state only once and derive results using memoization.
- **Space Complexity**: O(n) due to memoization table and recursion stack.

### Iterative Dynamic Programming

#### Intuition
The iterative DP solution efficiently calculates the results using arrays where each entry corresponds to the max possible profit at each day given different states of having stocks or not. We define `buy`, `sell`, and `cooldown` arrays where each index `i` keeps track of max profit at day `i`. Using predefined transitions between these states, we reduce the redundant computations to compute the result iteratively.

#### Code
```go
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
        buy[i] = max(buy[i-1], cooldown[i-1]-prices[i])
        sell[i] = buy[i-1] + prices[i]
        cooldown[i] = max(cooldown[i-1], sell[i-1])
    }
    
    return max(sell[n-1], cooldown[n-1])
}
```

#### Complexity
- **Time Complexity**: O(n) where n is the number of days because each day's states are computed using previously calculated results.
- **Space Complexity**: O(n) due to storing profit states for each day.

