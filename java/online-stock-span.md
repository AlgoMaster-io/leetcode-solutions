# [Leetcode 901: Online Stock Span](https://leetcode.com/problems/online-stock-span/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Stack for Efficient Span Calculation](#approach-2-stack-for-efficient-span-calculation)

---

## Approach 1: Brute Force

### Intuition
The brute force approach involves keeping track of all the stock prices encountered so far and then, for each new price, iterating backwards through the stored prices to calculate the span directly. This means, for each price, we need to traverse backwards until a price higher than the current price is found.

### Algorithm
1. Initialize a list to store the stock prices.
2. For each new price, traverse backwards in the list:
   - Increase the span as long as prices are less than or equal to the current price.
3. Return the span once a higher price is found or the list is exhausted.

### Code
```java
import java.util.ArrayList;
import java.util.List;

class StockSpanner {
    private List<Integer> prices;

    public StockSpanner() {
        prices = new ArrayList<>();
    }

    public int next(int price) {
        prices.add(price);
        int span = 1;
        int index = prices.size() - 2;

        // Traverse backwards through the list
        while (index >= 0 && prices.get(index) <= price) {
            span++;
            index--;
        }

        return span;
    }
}
```

### Complexity Analysis
- **Time Complexity:** O(n^2) in the worst case for `n` calls to the `next` method because we might need to traverse all previous prices for each call.
- **Space Complexity:** O(n) for storing all the prices.

---

## Approach 2: Stack for Efficient Span Calculation

### Intuition
To optimize the previous approach, we can utilize a stack to maintain a history of price indices that help in quickly determining the span. We store prices along with their respective spans in a stack. Each time a price is added, all prices less than or equal to the current price are popped from the stack, effectively calculating the span in constant amortized time.

### Algorithm
1. Use a stack to store pairs of prices and their spans.
2. For each incoming price:
   - Pop elements from the stack while they are less than or equal to the current price and accumulate their spans.
   - The span for the current price is the accumulated span plus one (for the current price itself).
3. Push the current price and its calculated span onto the stack.

### Code
```java
import java.util.Stack;

class StockSpanner {
    private Stack<int[]> stack;

    public StockSpanner() {
        stack = new Stack<>();
    }

    public int next(int price) {
        int span = 1;

        // Pop elements with a price less than or equal to the current price
        while (!stack.isEmpty() && stack.peek()[0] <= price) {
            span += stack.pop()[1];
        }

        stack.push(new int[]{price, span});

        return span;
    }
}
```

### Complexity Analysis
- **Time Complexity:** O(1) on average for each call to `next`, because each element is pushed and popped from the stack at most once.
- **Space Complexity:** O(n) for storing elements in the stack where `n` is the number of prices recorded.

