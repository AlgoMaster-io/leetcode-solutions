# [Leetcode 122: Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

## Approaches
1. [Greedy Approach](#greedy-approach)
2. [Peak Valley Approach](#peak-valley-approach)

## Greedy Approach

### Intuition
The key insight for this problem is realizing that you can "buy" and "sell" on the same day if it helps to accumulate profit. This boils down to taking advantage of every increase in stock price. For each day, if the price of the stock is higher than the previous day, it would be beneficial to treat the transaction from the previous day to the current day as a buy-sell pair to realize that gain.

### Algorithm
- Loop through each day's stock prices.
- If today's price is higher than yesterday's, add the difference to the total profit.
- Continue this until the end of the price list.

### Complexity Analysis
- **Time Complexity**: \(O(n)\), where \(n\) is the number of days of stock prices. We only traverse the list once.
- **Space Complexity**: \(O(1)\), only a constant amount of space is used.

### C# Code

```csharp
public class Solution {
    public int MaxProfit(int[] prices) {
        int maxProfit = 0;
        
        // Iterate over the prices starting from the second day
        for (int i = 1; i < prices.Length; i++) {
            // If today's price is greater than yesterday's, a profitable transaction can ensue
            if (prices[i] > prices[i - 1]) {
                // Accumulate the profit
                maxProfit += prices[i] - prices[i - 1];
            }
        }

        return maxProfit;
    }
}
```

## Peak Valley Approach

### Intuition
This approach is similar to the Greedy approach but a bit more structured in identifying the actual peaks and valleys. The idea is to track every time we go from a valley (local minimum) to a peak (local maximum) and consider this entire ascent as one buy-sell transaction.

### Algorithm
- Initialize a profit variable to track total profit.
- Traverse the price list and look for changes in prices:
  - Buy at the identified valley (local minimum).
  - Sell at the identified peak (local maximum).
- For each transaction, accumulate the difference between the peak and the valley in the total profit.

### Complexity Analysis
- **Time Complexity**: \(O(n)\), where \(n\) is the number of days of stock prices. We traverse the list once.
- **Space Complexity**: \(O(1)\), we use a constant amount of extra space.

### C# Code

```csharp
public class Solution {
    public int MaxProfit(int[] prices) {
        int i = 0;
        int n = prices.Length;
        int maxProfit = 0;
        
        // Loop until the end of price array is reached
        while (i < n - 1) {
            // Find the lowest price to buy (local minimum)
            while (i < n - 1 && prices[i] >= prices[i + 1]) {
                i++;
            }
            int valley = prices[i];
            
            // Find the highest price to sell (local maximum)
            while (i < n - 1 && prices[i] <= prices[i + 1]) {
                i++;
            }
            int peak = prices[i];
            
            // Add the profit of this transaction
            maxProfit += peak - valley;
        }
        
        return maxProfit;
    }
}
```


