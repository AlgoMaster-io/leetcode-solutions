# 2034. [Stock Price Fluctuation](https://leetcode.com/problems/stock-price-fluctuation/)

## Approach: HashMap and SortedDictionary

### Solution
csharp
```csharp
// Time Complexity:
//   - Update(): O(log n)
//   - Current(): O(1)
//   - Maximum(): O(1)
//   - Minimum(): O(1)
// Space Complexity: O(n), where n is the number of unique timestamps
using System;
using System.Collections.Generic;

public class StockPrice {
    private int latestTimestamp;
    private Dictionary<int, int> timestampPriceMap;
    private SortedDictionary<int, int> priceFrequencyMap;

    public StockPrice() {
        latestTimestamp = 0;
        timestampPriceMap = new Dictionary<int, int>();
        priceFrequencyMap = new SortedDictionary<int, int>();
    }

    // Update the stock price at a given timestamp
    public void Update(int timestamp, int price) {
        latestTimestamp = Math.Max(latestTimestamp, timestamp);

        // If the timestamp already exists, adjust the price frequency
        if (timestampPriceMap.ContainsKey(timestamp)) {
            int oldPrice = timestampPriceMap[timestamp];
            priceFrequencyMap[oldPrice] -= 1;
            if (priceFrequencyMap[oldPrice] == 0) {
                priceFrequencyMap.Remove(oldPrice);
            }
        }

        // Update the price for the timestamp
        timestampPriceMap[timestamp] = price;
        if (!priceFrequencyMap.ContainsKey(price)) {
            priceFrequencyMap[price] = 0;
        }
        priceFrequencyMap[price] += 1;
    }

    // Return the latest price
    public int Current() {
        return timestampPriceMap[latestTimestamp];
    }

    // Return the maximum price
    public int Maximum() {
        return priceFrequencyMap.Keys.Max();
    }

    // Return the minimum price
    public int Minimum() {
        return priceFrequencyMap.Keys.Min();
    }
}
```

