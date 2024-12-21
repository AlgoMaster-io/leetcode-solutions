## [Leetcode 901: Online Stock Span](https://leetcode.com/problems/online-stock-span/)

### Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Using Stack for Optimization](#using-stack-for-optimization)

---

### Brute Force Approach

Intuition:
The simplest way to solve this problem is to keep track of the prices encountered so far in a list. For each price, we traverse backwards through the collected list to count the span of how many consecutive days (including today) the price has been less than or equal to today’s price.

#### Steps:
1. Store each day's price as it arrives.
2. For each new price, iterate over the stored prices from the most recent day backward.
3. Count how many consecutive days have a price less than or equal to the current price.

#### JavaScript Code:
```javascript
class StockSpanner {
    constructor() {
        this.prices = [];
    }
    
    next(price) {
        this.prices.push(price);
        let i = this.prices.length - 1;
        let span = 1;
        
        // Starting from the current index, go backwards and count consecutive lesser prices
        while (i - 1 >= 0 && this.prices[i - 1] <= price) {
            span++;
            i--;
        }
        
        return span;
    }
}
```

#### Complexity Analysis:
- **Time Complexity:** O(n) for each call to `next(price)`, where n is the number of days seen so far.
- **Space Complexity:** O(n), storing each of the prices.

---

### Using Stack for Optimization

Intuition:
To optimize the earlier approach, we can use a stack that keeps track of pairs of (stock price, span). The stack allows us to "flatten" previous consecutive days with prices less than or equal to the current day's price by keeping only the cumulative span for those days.

#### Steps:
1. Use a stack to store pairs `(price, span)` for each day's price.
2. For a new price, pop from the stack all previous prices that are less than or equal to the current price, while summing up their spans.
3. Push the current price and its span onto the stack.

#### JavaScript Code:
```javascript
class StockSpanner {
    constructor() {
        // Stack stores pairs of (price, span)
        this.stack = [];
    }
    
    next(price) {
        let span = 1;  // Initialize current day's span to 1

        // Merge spans of prices that are less than or equal to the current price
        while (this.stack.length && this.stack[this.stack.length - 1][0] <= price) {
            span += this.stack.pop()[1];
        }

        // Push the price and its cumulative span onto the stack
        this.stack.push([price, span]);
        
        return span;
    }
}
```

#### Complexity Analysis:
- **Time Complexity:** Average O(1) per call to `next(price)`, as each price is pushed and popped at most once from the stack.
- **Space Complexity:** O(n), as at worst, all prices can be strictly increasing requiring storage of each entry in the stack.

In this optimized solution, we achieve a much better average time complexity by using a stack to simplify the accumulation of spans for consecutive days with lower or equal prices.

