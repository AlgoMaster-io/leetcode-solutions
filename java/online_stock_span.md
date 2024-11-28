# [901. Online Stock Span](https://leetcode.com/problems/online-stock-span/)

## Approach 1: Brute Force

### Solution
```java
// Time Complexity: O(n^2) in the worst case
// Space Complexity: O(n)
public class StockSpanner {
    private List<Integer> prices;
    private List<Integer> spans;

    public StockSpanner() {
        prices = new ArrayList<>();
        spans = new ArrayList<>();
    }

    public int next(int price) {
        int span = 1;
        // Check previous prices for consecutive smaller or equal prices
        for (int i = prices.size() - 1; i >= 0 && prices.get(i) <= price; i--) {
            span += spans.get(i);
        }
        prices.add(price);
        spans.add(span);
        return span;
    }
}
```

## Approach 2: Using a Stack (Optimal Solution)

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
import java.util.Stack;

public class StockSpanner {
    private Stack<int[]> stack; // Stack stores pairs of {price, span}

    public StockSpanner() {
        stack = new Stack<>();
    }

    public int next(int price) {
        int span = 1;

        // Combine spans of all previous smaller or equal prices
        while (!stack.isEmpty() && stack.peek()[0] <= price) {
            span += stack.pop()[1];
        }

        stack.push(new int[]{price, span}); // Push the current price and its span
        return span;
    }
}
```