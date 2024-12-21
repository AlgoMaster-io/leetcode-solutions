[Leetcode 122: Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

Approaches:
- [Greedy Solution](#greedy-solution)
- [Peak Valley Approach](#peak-valley-approach)

### Greedy Solution
The problem is essentially about finding opportunities to make as much profit as possible by buying stocks on one day and selling them on another. This can be achieved by making transactions whenever there's a profit to be made, without concern for a future decrement in prices. 

#### Intuition:
In a simplified manner, the problem reduces to accumulating all positive differences between consecutive days. By always projecting into the future and accumulating possible gains daily, we capitalize on every rising curve of the stock price graph. 

#### Java Code:
```java
public class StockTrading {
    public int maxProfit(int[] prices) {
        int maxProfit = 0;
        
        for (int i = 1; i < prices.length; i++) {
            // If the current price is higher than the previous day's price, we have a profit opportunity:
            if (prices[i] > prices[i - 1]) {
                // Accumulate the profit by subtracting yesterday's price from today's price.
                maxProfit += prices[i] - prices[i - 1];
            }
        }
        
        // At the end, maxProfit holds the total profit accrued through all transactions.
        return maxProfit;
    }
}
```
#### Time Complexity:
- **O(n)**: Iterate through the prices array once.
#### Space Complexity:
- **O(1)**: Uses constant space.

### Peak Valley Approach
Another way to look at this problem is to find every consecutive pair of peaks and valleys. A peak-to-valley traversal adds the difference from each peak to its preceding valley to determine the overall profit through multiple transactions.

#### Intuition:
This approach mimics the real-world scenario of buying stock at valleys (local minimums) and selling it at peaks (local maximums). This enables capturing every increasing sequence even if the overall curve spans over several days.

#### Java Code:
```java
public class StockTrading {
    public int maxProfit(int[] prices) {
        int i = 0;
        int maxProfit = 0;
        int valley = prices[0];
        int peak = prices[0];
        
        while (i < prices.length - 1) {
            // Move the index to the valley/local minimum
            while (i < prices.length - 1 && prices[i] >= prices[i + 1]) {
                i++;
            }
            valley = prices[i];
            
            // Move the index to the peak/local maximum
            while (i < prices.length - 1 && prices[i] <= prices[i + 1]) {
                i++;
            }
            peak = prices[i];
            
            // Add the difference between peak and valley
            maxProfit += peak - valley;
        }
        
        // maxProfit now contains the total profit from valley to peak differences
        return maxProfit;
    }
}
```
#### Time Complexity:
- **O(n)**: Traverses the list and finds peaks and valleys.
#### Space Complexity:
- **O(1)**: Uses constant space for computation.

Both solutions efficiently compute profits by capturing profitable intervals, but the first (greedy solution) is generally more succinct and widely preferred due to its straightforward implementation.

