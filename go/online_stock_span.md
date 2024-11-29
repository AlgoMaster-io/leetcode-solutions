# [901. Online Stock Span](https://leetcode.com/problems/online-stock-span/)

## Approach 1: Brute Force

### Solution
go
```go
// Time Complexity: O(n^2) in the worst case
// Space Complexity: O(n)
type StockSpanner struct {
    prices []int
    spans  []int
}

func Constructor() StockSpanner {
    return StockSpanner{
        prices: make([]int, 0),
        spans:  make([]int, 0),
    }
}

func (this *StockSpanner) Next(price int) int {
    span := 1
    // Check previous prices for consecutive smaller or equal prices
    for i := len(this.prices) - 1; i >= 0 && this.prices[i] <= price; i-- {
        span += this.spans[i]
    }
    this.prices = append(this.prices, price)
    this.spans = append(this.spans, span)
    return span
}
```

## Approach 2: Using a Stack (Optimal Solution)

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
type StockSpanner struct {
    stack [][2]int // Stack stores pairs of {price, span}
}

func Constructor() StockSpanner {
    return StockSpanner{
        stack: make([][2]int, 0),
    }
}

func (this *StockSpanner) Next(price int) int {
    span := 1

    // Combine spans of all previous smaller or equal prices
    for len(this.stack) > 0 && this.stack[len(this.stack)-1][0] <= price {
        span += this.stack[len(this.stack)-1][1]
        this.stack = this.stack[:len(this.stack)-1]
    }

    this.stack = append(this.stack, [2]int{price, span}) // Push the current price and its span
    return span
}
```

