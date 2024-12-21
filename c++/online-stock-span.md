## [Online Stock Span - LeetCode](https://leetcode.com/problems/online-stock-span/)

### Approaches:
1. [Brute Force Approach](#approach-1-brute-force-approach)
2. [Stack Optimization](#approach-2-stack-optimization)

---

### Approach 1: Brute Force Approach

The brute force approach iterates through all the previous stock prices every time a new stock price is added. This ensures the span is calculated by counting how many consecutive days, including today, the stock price was less than or equal to today's price.

#### Intuition:
- For each price, we start from that price and move backwards to check how many consecutive prices before it are less than or equal to it.
- Although this approach will work, it's inefficient because for each new price, we might go back to the very beginning.

#### Code:
```cpp
class StockSpanner {
public:
    vector<int> prices;

    StockSpanner() {}

    int next(int price) {
        prices.push_back(price);
        
        int span = 1; // Initialize span as 1 for the current price itself
        for (int i = prices.size() - 2; i >= 0; --i) {
            // Increment span if previous prices are less than or equal to the current price
            if (prices[i] <= price) {
                ++span;
            } else {
                break; // Break when a higher price is found
            }
        }
        return span;
    }
};
```

#### Time Complexity:
- **O(N^2)** for each price where N is the number of stock prices seen so far (in the worst case when in descending order).

#### Space Complexity:
- **O(N)** for storing all prices.

---

### Approach 2: Stack Optimization

In order to optimize, maintain a stack that keeps track of prices along with their calculated span, ensuring efficient access and manipulation.

#### Intuition:
- The stack is used to maintain a decreasing sequence of prices.
- While adding a new price, pop prices from the stack that are lesser or equal to the new price and accumulate their spans.
- By doing so, we ensure that for any price, we account for all previous smaller or equal prices without revisiting them.

#### Code:
```cpp
class StockSpanner {
public:
    stack<pair<int, int>> st; // Pair of price and its calculated span

    StockSpanner() {}

    int next(int price) {
        int span = 1;

        // Accumulate spans for prices less or equal to current price
        while (!st.empty() && st.top().first <= price) {
            span += st.top().second;
            st.pop();
        }

        // Push current price and its span to the stack
        st.push({price, span});

        return span;
    }
};
```

#### Time Complexity:
- **O(N)** for each price on average, due to potential all stack elements popping.

#### Space Complexity:
- **O(N)** for storing price spans in the stack.

With these approaches, the problem offers a way to learn the importance of data structures like stacks in simplifying and optimizing seemingly complex problems.

