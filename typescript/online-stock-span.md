# [Leetcode 901: Online Stock Span](https://leetcode.com/problems/online-stock-span/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Monotonic Stack](#approach-2-monotonic-stack)

---

## Approach 1: Brute Force

### Intuition
The problem asks us to compute the stock span of the ith day, defined as the number of consecutive days before and including the ith day, for which the stock price does not exceed the price of the ith day. A straightforward approach is to, for each incoming price, iterate backwards until we find a price that is greater than the current price.

### Steps
1. Maintain an array to store all incoming prices.
2. For every incoming price, start a backward loop from that price and count the number of days up to a day where the price was higher than the current day’s price.

### Code
```typescript
class StockSpanner {
    private prices: number[];
    
    constructor() {
        this.prices = [];
    }

    next(price: number): number {
        this.prices.push(price);
        // Current price's index
        let span = 1;
        for (let i = this.prices.length - 2; i >= 0; i--) {
            if (this.prices[i] <= price) {
                span++;
            } else {
                break;
            }
        }
        return span;
    }
}
```

### Time Complexity
- **Insertion:** O(n) in the worst case.
- **Retrieval:** O(n) in the worst case, where n is the number of prices added so far.
  
### Space Complexity
- O(n) to store the prices.

---

## Approach 2: Monotonic Stack

### Intuition
To improve the efficiency of our solution, we can use a monotonic stack that keeps track of indices of stock prices in a non-increasing order. The idea is to skip as many indices as possible that have already been processed and are less than the current price.

### Steps
1. Use a stack that stores pairs of indices and their corresponding stock prices.
2. For each new price, pop elements from the stack until the elements at those indices are greater than the current price.
3. The difference between the current index and the index at the top of the stack gives us the span.

### Code
```typescript
class StockSpanner {
    private stack: Array<[number, number]>; // (index, price)
    private index: number;
    
    constructor() {
        this.stack = [];
        this.index = -1;
    }

    next(price: number): number {
        this.index++; // Increment our current index to account for a new day.
        
        // Keep popping elements from stack till we find one that is greater.
        // Which means the current price spans all these elements.
        while (this.stack.length > 0 && this.stack[this.stack.length - 1][1] <= price) {
            this.stack.pop();
        }

        const span = this.stack.length > 0 ? this.index - this.stack[this.stack.length - 1][0] : this.index + 1;

        // Push the current price and index onto the stack.
        this.stack.push([this.index, price]);

        return span;
    }
}
```

### Time Complexity
- **Insertion:** O(1) on average. Each price is pushed and popped at most once.

### Space Complexity
- O(n) for maintaining the monotonic stack.

