# Online Stock Span
Design an algorithm that collects daily price quotes for some stock and returns the span of that stock's price for the current day.

The span of the stock's price today is defined as the maximum number of consecutive days (starting from today and going backward) for which the price of the stock was less than or equal to today’s price.

Implement the StockSpanner class:
- StockSpanner() initializes the object of the class.
- int next(int price) returns the span of the stock's price for the current day.

### Constraints:
- 1 <= price <= 10^5
- At most 10^4 calls will be made to next.

### Examples
```javascript
Input:
["StockSpanner", "next", "next", "next", "next", "next", "next", "next"]
[[], [100], [80], [60], [70], [60], [75], [85]]

Output:
[null, 1, 1, 1, 2, 1, 4, 6]

Explanation:
StockSpanner stockSpanner = new StockSpanner();
stockSpanner.next(100); // return 1
stockSpanner.next(80);  // return 1
stockSpanner.next(60);  // return 1
stockSpanner.next(70);  // return 2
stockSpanner.next(60);  // return 1
stockSpanner.next(75);  // return 4
stockSpanner.next(85);  // return 6
```

## Approaches to Solve the Problem
### Approach 1: Brute Force (Inefficient)
##### Intuition:
For each day's price, you can iterate backward over all previous prices and count how many consecutive days had prices that were less than or equal to the current price. While simple, this approach is inefficient because it involves checking each day's price every time.

Steps:
1. Initialize an empty list to store the prices.
2. For each new price, iterate backward through the list and count how many consecutive days had prices less than or equal to the current price.
3. Return the span count.
##### Time Complexity:
O(n²), where n is the number of price quotes. In the worst case, for each price, we iterate backward through all the previous prices.
##### Space Complexity:
O(n), for storing the prices.
##### Python Code:
```python
class StockSpanner:
    def __init__(self):
        self.prices = []

    def next(self, price: int) -> int:
        self.prices.append(price)
        span = 1
        i = len(self.prices) - 2
        while i >= 0 and self.prices[i] <= price:
            span += 1
            i -= 1
        return span
```

### Approach 2: Stack-based Solution (Optimal)
##### Intuition: 
We can optimize the brute-force solution by using a monotonic decreasing stack to store the prices and their respective spans. Instead of iterating over all previous prices, we can "skip" days where the price is smaller by maintaining a stack where each price is greater than the price of the next day.

- For each new price, we pop the stack until we find a price greater than the current price. The sum of the spans from the popped elements is the span of the current price.

Steps:
1. Initialize a stack to store tuples of (price, span).
2. For each new price, pop elements from the stack as long as the top of the stack contains a price less than or equal to the current price.
3. The span of the current price is 1 (itself) plus the sum of the spans of the popped elements.
4. Push the current price and its span onto the stack.
### Visualization
For the input:
prices = [100, 80, 60, 70, 60, 75, 85]

The stack processing looks like this:
```rust
Day 1 (100): stack = [(100, 1)] → span = 1
Day 2 (80): stack = [(100, 1), (80, 1)] → span = 1
Day 3 (60): stack = [(100, 1), (80, 1), (60, 1)] → span = 1
Day 4 (70): pop (60, 1), stack = [(100, 1), (80, 1)] → span = 2
Day 5 (60): stack = [(100, 1), (80, 1), (60, 1)] → span = 1
Day 6 (75): pop (60, 1), pop (70, 2), stack = [(100, 1), (80, 1)] → span = 4
Day 7 (85): pop (75, 4), pop (80, 1), stack = [(100, 1)] → span = 6
```
##### Time Complexity:
O(1) on average for each next() call. Each price is pushed and popped from the stack at most once, leading to amortized constant time.
##### Space Complexity:
O(n), where n is the number of prices, for storing the prices and spans on the stack.
##### Python Code:
```python
class StockSpanner:
    def __init__(self):
        # Stack to store pairs of (price, span)
        self.stack = []

    def next(self, price: int) -> int:
        span = 1
        
        # Pop elements from the stack while the current price is greater than or equal to stack top
        while self.stack and self.stack[-1][0] <= price:
            span += self.stack.pop()[1]
        
        # Push the current price and its span to the stack
        self.stack.append((price, span))
        
        return span
```
### Edge Cases:
1. Single Price: If there is only one price in the input, the span will always be 1 because no previous days exist.
2. Monotonically Increasing Prices: The span will increase for each new price, as each price is greater than the previous day.
3. Monotonically Decreasing Prices: The span will always be 1 for each price because no previous day had a smaller or equal price.
## Summary

| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Brute Force                        | O(n²)      | O(n)             |
| Stack-based Solution (Optimal)                          | O(1) on average            | O(n)             |

The Stack-based Solution is the most efficient, with an amortized time complexity of O(1) for each call to next(). It uses a monotonic stack to store the prices and their spans, allowing for constant-time lookups and updates.