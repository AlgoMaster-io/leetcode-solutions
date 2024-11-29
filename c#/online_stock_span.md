# [901. Online Stock Span](https://leetcode.com/problems/online-stock-span/)

## Approach 1: Brute Force

### Solution
csharp
```csharp
// Time Complexity: O(n^2) in the worst case
// Space Complexity: O(n)
using System.Collections.Generic;

public class StockSpanner {
    private List<int> prices;
    private List<int> spans;

    public StockSpanner() {
        prices = new List<int>();
        spans = new List<int>();
    }

    public int Next(int price) {
        int span = 1;
        // Check previous prices for consecutive smaller or equal prices
        for (int i = prices.Count - 1; i >= 0 && prices[i] <= price; i--) {
            span += spans[i];
        }
        prices.Add(price);
        spans.Add(span);
        return span;
    }
}
```

## Approach 2: Using a Stack (Optimal Solution)

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
using System.Collections.Generic;

public class StockSpanner {
    private Stack<int[]> stack; // Stack stores pairs of {price, span}

    public StockSpanner() {
        stack = new Stack<int[]>();
    }

    public int Next(int price) {
        int span = 1;

        // Combine spans of all previous smaller or equal prices
        while (stack.Count > 0 && stack.Peek()[0] <= price) {
            span += stack.Pop()[1];
        }

        stack.Push(new int[]{price, span}); // Push the current price and its span
        return span;
    }
}
```

