# [901. Online Stock Span](https://leetcode.com/problems/online-stock-span/)

## Approach 1: Brute Force

### Solution
```cpp
// Time Complexity: O(n^2) in the worst case
// Space Complexity: O(n)
#include <vector>

class StockSpanner {
private:
    std::vector<int> prices;
    std::vector<int> spans;

public:
    StockSpanner() {}

    int next(int price) {
        int span = 1;
        // Check previous prices for consecutive smaller or equal prices
        for (int i = prices.size() - 1; i >= 0 && prices[i] <= price; i--) {
            span += spans[i];
        }
        prices.push_back(price);
        spans.push_back(span);
        return span;
    }
};
```

## Approach 2: Using a Stack (Optimal Solution)

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
#include <stack>
#include <utility>

class StockSpanner {
private:
    std::stack<std::pair<int, int>> stack; // Stack stores pairs of {price, span}

public:
    StockSpanner() {}

    int next(int price) {
        int span = 1;

        // Combine spans of all previous smaller or equal prices
        while (!stack.empty() && stack.top().first <= price) {
            span += stack.top().second;
            stack.pop();
        }

        stack.push({price, span}); // Push the current price and its span
        return span;
    }
};
```

