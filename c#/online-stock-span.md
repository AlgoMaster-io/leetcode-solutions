# [LeetCode 901: Online Stock Span](https://leetcode.com/problems/online-stock-span/)

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Monotonic Stack Approach](#monotonic-stack-approach)

---

### Brute Force Approach

#### Intuition:
The brute force approach is straightforward: for each price given, traverse backwards and count how many consecutive previous days had a price that is less than or equal to the current day's price. This method checks each previous price, resulting in a simple but inefficient solution, especially as the number of prices grows.

#### Code:
```csharp
public class StockSpanner
{
    private List<int> prices;

    public StockSpanner()
    {
        prices = new List<int>();
    }
    
    public int Next(int price)
    {
        prices.Add(price);
        int span = 1;
        
        // Traverse backwards to calculate the span.
        for (int i = prices.Count - 2; i >= 0; i--)
        {
            if (prices[i] <= price)
            {
                span++;
            }
            else
            {
                break;
            }
        }
        
        return span;
    }
}
```

#### Time Complexity:
- **O(n^2)** where `n` is the number of elements processed so far.
- For each new price, we may check all previous prices.

#### Space Complexity:
- **O(n)** to keep the list of prices.

---

### Monotonic Stack Approach

#### Intuition:
To optimize, the key insight is using a monotonic stack to maintain a way to skip directly to the last seen price that is greater than the current price. Each element in the stack is a pair: the price and its stock span. When a new price is lower than the price at the top of the stack, we simply push it onto the stack; otherwise, we pop elements from the stack, accumulating their spans to form the span of the current price.

#### Code:
```csharp
public class StockSpanner
{
    private Stack<(int price, int span)> stack;

    public StockSpanner()
    {
        stack = new Stack<(int, int)>();
    }
    
    public int Next(int price)
    {
        int span = 1;
        
        // Pop elements from the stack while the current price is larger or equal
        // to the price at the top of the stack, incrementing the span accordingly.
        while (stack.Count > 0 && stack.Peek().price <= price)
        {
            span += stack.Pop().span;
        }
        
        // Push the current price and its calculated span onto the stack.
        stack.Push((price, span));
        
        return span;
    }
}
```

#### Time Complexity:
- **Amortized O(1)** for each `next` operation.
- Each element is pushed and popped from the stack at most once, resulting in amortized constant time per operation.

#### Space Complexity:
- **O(n)** given the constraint size, to store the stack elements. 

---

This concludes the solution walkthrough of LeetCode 901: Online Stock Span. The transition from brute force to using a monotonic stack significantly optimizes both time complexity and performance.

