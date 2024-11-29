# [901. Online Stock Span](https://leetcode.com/problems/online-stock-span/)

## Approach 1: Brute Force

### Solution
python
```python
# Time Complexity: O(n^2) in the worst case
# Space Complexity: O(n)
class StockSpanner:
    def __init__(self):
        self.prices = []
        self.spans = []

    def next(self, price: int) -> int:
        span = 1
        # Check previous prices for consecutive smaller or equal prices
        for i in range(len(self.prices) - 1, -1, -1):
            if self.prices[i] > price:
                break
            span += self.spans[i]
        
        self.prices.append(price)
        self.spans.append(span)
        return span
```

## Approach 2: Using a Stack (Optimal Solution)

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
class StockSpanner:
    def __init__(self):
        self.stack = []  # Stack stores pairs of (price, span)

    def next(self, price: int) -> int:
        span = 1
        
        # Combine spans of all previous smaller or equal prices
        while self.stack and self.stack[-1][0] <= price:
            span += self.stack.pop()[1]
        
        self.stack.append((price, span))  # Push the current price and its span
        return span
```

