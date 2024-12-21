# [Leetcode 901: Online Stock Span](https://leetcode.com/problems/online-stock-span/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Optimal Stack Approach](#optimal-stack-approach)

---

### Brute Force Approach

**Intuition:**

The brute force approach involves maintaining a list of stock prices that we iterate over to determine the span whenever a new price is added. For each new stock price, we compare it with all previous prices in reverse order until we find a price that is greater than the current price or we reach the beginning of the list. The span will be the count of these prices plus one.

**Steps:**

1. Maintain a list to store stock prices received so far.
2. For each new stock price, start checking from the end of the list and count how far back you can go before hitting a stock price greater than the current price.
3. Return the count of prices looked over, representing the span.

**Time Complexity:** O(N^2) for N stock prices, since each new price addition involves potentially iterating over all previous prices.

**Space Complexity:** O(N), required to store the prices.

```go
type StockSpanner struct {
    prices []int // List to keep track of all the stock prices
}

func Constructor() StockSpanner {
    return StockSpanner{prices: []int{}}
}

func (this *StockSpanner) Next(price int) int {
    this.prices = append(this.prices, price) // Add the new price
    
    span := 1 // Start with a span of 1 (the current day)
    // Move backward through the list to count the span
    for i := len(this.prices) - 2; i >= 0 && this.prices[i] <= price; i-- {
        span++ // Increment span if previous prices are less than or equal
    }
    
    return span
}
```

---

### Optimal Stack Approach

**Intuition:**

To optimize the time complexity, we can make use of a stack. The stack maintains pairs of the stock price and the span of stock prices that have been processed, ensuring O(1) average time complexity per `Next` operation.

1. Initialize an empty stack.
2. For each new price, pop from the stack while the top of the stack is less than or equal to the new price. This effectively adds the spans of popped elements to the current span.
3. Push the current price with its calculated span onto the stack.
4. The stack allows us to 'remember' not just prices, but how far back each price affects the span without checking each individual price again.

**Time Complexity:** O(N) amortized per total of N calls to `Next`, as each price is pushed and popped from the stack exactly once.

**Space Complexity:** O(N) in the worst case, where each price is added to the stack.

```go
type StockSpanner struct {
    stack [][2]int // Stack to store pairs of (price, span)
}

func Constructor() StockSpanner {
    return StockSpanner{stack: [][2]int{}}
}

func (this *StockSpanner) Next(price int) int {
    span := 1 // Start with a default span of 1 (the current price itself)
    // Process the stack to find the effective span for the current price
    for len(this.stack) > 0 && this.stack[len(this.stack)-1][0] <= price {
        // Add up spans of prices that are less than or equal to the current price
        span += this.stack[len(this.stack)-1][1]
        this.stack = this.stack[:len(this.stack)-1] // Pop the stack
    }
    // Push the current price and its calculated span onto the stack
    this.stack = append(this.stack, [2]int{price, span})
    return span
}
```

This solution efficiently manages the calculation of stock spans by utilizing a stack structure that both simplifies the process and accelerates the time complexity compared to the brute force approach.

