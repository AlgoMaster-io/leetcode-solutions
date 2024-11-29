# 2034. [Stock Price Fluctuation](https://leetcode.com/problems/stock-price-fluctuation/)

## Approach: HashMap and Two Heaps

### Solution
```java
// Time Complexity:
//   - update(): O(log n)
//   - current(): O(1)
//   - maximum(): O(1)
//   - minimum(): O(1)
// Space Complexity: O(n), where n is the number of unique timestamps
import java.util.HashMap;
import java.util.TreeMap;

public class StockPrice {

    private int latestTimestamp;
    private HashMap<Integer, Integer> timestampPriceMap;
    private TreeMap<Integer, Integer> priceFrequencyMap;

    public StockPrice() {
        latestTimestamp = 0;
        timestampPriceMap = new HashMap<>();
        priceFrequencyMap = new TreeMap<>();
    }

    // Update the stock price at a given timestamp
    public void update(int timestamp, int price) {
        latestTimestamp = Math.max(latestTimestamp, timestamp);

        // If the timestamp already exists, adjust the price frequency
        if (timestampPriceMap.containsKey(timestamp)) {
            int oldPrice = timestampPriceMap.get(timestamp);
            priceFrequencyMap.put(oldPrice, priceFrequencyMap.get(oldPrice) - 1);
            if (priceFrequencyMap.get(oldPrice) == 0) {
                priceFrequencyMap.remove(oldPrice);
            }
        }

        // Update the price for the timestamp
        timestampPriceMap.put(timestamp, price);
        priceFrequencyMap.put(price, priceFrequencyMap.getOrDefault(price, 0) + 1);
    }

    // Return the latest price
    public int current() {
        return timestampPriceMap.get(latestTimestamp);
    }

    // Return the maximum price
    public int maximum() {
        return priceFrequencyMap.lastKey();
    }

    // Return the minimum price
    public int minimum() {
        return priceFrequencyMap.firstKey();
    }
}
```