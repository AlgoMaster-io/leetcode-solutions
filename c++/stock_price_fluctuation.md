# 2034. [Stock Price Fluctuation](https://leetcode.com/problems/stock-price-fluctuation/)

## Approach: HashMap and Two Heaps

### Solution
```cpp
// Time Complexity:
//   - update(): O(log n)
//   - current(): O(1)
//   - maximum(): O(1)
//   - minimum(): O(1)
// Space Complexity: O(n), where n is the number of unique timestamps
#include <unordered_map>
#include <map>

class StockPrice {
private:
    int latestTimestamp;
    std::unordered_map<int, int> timestampPriceMap;
    std::map<int, int> priceFrequencyMap;

public:
    StockPrice() : latestTimestamp(0) {}

    // Update the stock price at a given timestamp
    void update(int timestamp, int price) {
        latestTimestamp = std::max(latestTimestamp, timestamp);

        // If the timestamp already exists, adjust the price frequency
        if (timestampPriceMap.count(timestamp)) {
            int oldPrice = timestampPriceMap[timestamp];
            priceFrequencyMap[oldPrice]--;
            if (priceFrequencyMap[oldPrice] == 0) {
                priceFrequencyMap.erase(oldPrice);
            }
        }

        // Update the price for the timestamp
        timestampPriceMap[timestamp] = price;
        priceFrequencyMap[price]++;
    }

    // Return the latest price
    int current() const {
        return timestampPriceMap.at(latestTimestamp);
    }

    // Return the maximum price
    int maximum() const {
        return priceFrequencyMap.rbegin()->first;
    }

    // Return the minimum price
    int minimum() const {
        return priceFrequencyMap.begin()->first;
    }
};
```

