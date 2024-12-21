# [Leetcode 901: Online Stock Span](https://leetcode.com/problems/online-stock-span/)

## Approaches

1. [Brute Force Solution](#approach-1-brute-force-solution)
2. [Stack Optimization](#approach-2-stack-optimization)

---

### Approach 1: Brute Force Solution

#### Intuition
In the brute force approach, we aim to compute the stock span for each new price by iterating over all previous prices to check if they are less than or equal to the current price. This directly simulates the definition of span as the number of consecutive days where the price is less than or equal to the current day's price.

#### Steps
- Maintain a list to store all prices as they are input.
- For each new price, calculate the span by counting backwards until a price greater than the current price is found.
- Return the span count.

#### Code

```python
class StockSpanner:

    def __init__(self):
        self.prices = []  # Store past prices
        
    def next(self, price: int) -> int:
        self.prices.append(price)  # Add the new price to the list
        span = 0
        # Iterate from the most recent price backwards
        for i in range(len(self.prices) - 1, -1, -1):
            if self.prices[i] <= price:
                span += 1  # Count when current price is greater or equal to past price
            else:
                break  # Stop when we hit a higher price
        return span
```

#### Complexity Analysis
- **Time Complexity**: \(O(n^2)\) for n calls to `next()` due to the loop over past prices.
- **Space Complexity**: \(O(n)\) to store the prices.

---

### Approach 2: Stack Optimization

#### Intuition
The brute force solution can be inefficient as it repeatedly checks many previous prices. By utilizing a stack, we can efficiently compute spans in constant time by keeping track of prices and their corresponding spans. The key observation is that when a new price is added:
- If the new price is greater than or equal to the price at the top of the stack, it spans over it, and potentially spans over more preceding prices, which have already been evaluated and tracked via their span values.

#### Steps
- Use a stack where each element is a tuple of `(price, span)`.
- For each new price:
  - While the stack is not empty and the price on top of the stack is less than or equal to the current price, pop the stack and add its span to a cumulative span variable.
  - Push the current price and its computed span onto the stack.
- Return the total span calculated for the current price.

#### Code

```python
class StockSpanner:

    def __init__(self):
        self.stack = []  # Stack to store (price, span)

    def next(self, price: int) -> int:
        span = 1  # Start with a span of 1 (itself)
        # Pop elements that are less or equal to current price and accumulate their spans
        while self.stack and self.stack[-1][0] <= price:
            span += self.stack.pop()[1]
        # Push the current price along with its computed span to the stack
        self.stack.append((price, span))
        return span
```

#### Complexity Analysis
- **Time Complexity**: \(O(1)\) amortized for each call to `next()`. Each price is pushed and popped from the stack at most once.
- **Space Complexity**: \(O(n)\) for storing the stack, where n is the number of calls to `next()`.

