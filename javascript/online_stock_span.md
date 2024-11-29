# [901. Online Stock Span](https://leetcode.com/problems/online-stock-span/)

## Approach 1: Brute Force

### Solution
```javascript
// Time Complexity: O(n^2) in the worst case
// Space Complexity: O(n)
class StockSpanner {
    constructor() {
        this.prices = [];
        this.spans = [];
    }

    next(price) {
        let span = 1;
        // Check previous prices for consecutive smaller or equal prices
        for (let i = this.prices.length - 1; i >= 0 && this.prices[i] <= price; i--) {
            span += this.spans[i];
        }
        this.prices.push(price);
        this.spans.push(span);
        return span;
    }
}
```

## Approach 2: Using a Stack (Optimal Solution)

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(n)
class StockSpanner {
    constructor() {
        this.stack = []; // Stack stores pairs of [price, span]
    }

    next(price) {
        let span = 1;

        // Combine spans of all previous smaller or equal prices
        while (this.stack.length > 0 && this.stack[this.stack.length - 1][0] <= price) {
            span += this.stack.pop()[1];
        }

        this.stack.push([price, span]); // Push the current price and its span
        return span;
    }
}
```

