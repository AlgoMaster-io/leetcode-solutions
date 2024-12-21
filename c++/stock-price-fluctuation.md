# [LeetCode Problem 2034: Stock Price Fluctuation](https://leetcode.com/problems/stock-price-fluctuation/)

## Table of Contents
1. [Approach 1: Brute Force Using a Map](#approach-1)
2. [Approach 2: Using a Balanced Tree with Multisets](#approach-2)

---

## Approach 1: Brute Force Using a Map <a name="approach-1"></a>

### Intuition
The stock price fluctuation problem requires us to always know the latest stock price and the global maximum and minimum prices from all the timestamps. One straightforward approach is to use a map to store all stock prices with their timestamps and update the values accordingly. Whenever we need to retrieve the maximum, minimum, or latest price, we can iterate through the map to find these values.

### Code
```cpp
#include <iostream>
#include <map>
#include <climits>

class StockPrice {
private:
    std::map<int, int> timestampPriceMap;
    int maxTimestamp;

public:
    StockPrice() : maxTimestamp(0) {}

    void update(int timestamp, int price) {
        // Store the price for the given timestamp
        timestampPriceMap[timestamp] = price;
        // Update the maxTimestamp if this timestamp is the latest
        if (timestamp > maxTimestamp) {
            maxTimestamp = timestamp;
        }
    }

    int current() {
        // Return the price at the latest timestamp
        return timestampPriceMap[maxTimestamp];
    }

    int maximum() {
        int maxPrice = INT_MIN;
        // Iterate over all timestamp-price pairs to find the max price
        for (const auto& pair : timestampPriceMap) {
            if (pair.second > maxPrice) {
                maxPrice = pair.second;
            }
        }
        return maxPrice;
    }

    int minimum() {
        int minPrice = INT_MAX;
        // Iterate over all timestamp-price pairs to find the min price
        for (const auto& pair : timestampPriceMap) {
            if (pair.second < minPrice) {
                minPrice = pair.second;
            }
        }
        return minPrice;
    }
};
```

### Complexity
- **Time Complexity**: 
  - `update`, `current`: O(1)
  - `maximum`, `minimum`: O(n) where n is the number of timestamps stored.
- **Space Complexity**: O(n) for storing the stock price map.

---

## Approach 2: Using a Balanced Tree with Multisets <a name="approach-2"></a>

### Intuition
A more optimal solution is to use a combination of a map for storing prices with timestamps and a multiset for efficiently maintaining the maximum and minimum prices. The map helps in quickly updating or fetching the latest price, while the multiset allows us to maintain a sorted order of prices, making it easy to retrieve the minimum and maximum prices in constant time.

### Code
```cpp
#include <iostream>
#include <map>
#include <set>

class StockPrice {
private:
    std::map<int, int> timestampPriceMap;
    std::multiset<int> prices;
    int maxTimestamp;

public:
    StockPrice() : maxTimestamp(0) {}

    void update(int timestamp, int price) {
        // Check if the timestamp already exists
        if (timestampPriceMap.find(timestamp) != timestampPriceMap.end()) {
            // Remove the old price from the multiset
            prices.erase(prices.find(timestampPriceMap[timestamp]));
        }
        // Update the map with the new price
        timestampPriceMap[timestamp] = price;
        // Insert the new price into the multiset
        prices.insert(price);
        // Update the maxTimestamp if this timestamp is the latest
        if (timestamp > maxTimestamp) {
            maxTimestamp = timestamp;
        }
    }

    int current() {
        // Return the price at the latest timestamp
        return timestampPriceMap[maxTimestamp];
    }

    int maximum() {
        // Retrieve the maximum price from the multiset
        return *prices.rbegin();
    }

    int minimum() {
        // Retrieve the minimum price from the multiset
        return *prices.begin();
    }
};
```

### Complexity
- **Time Complexity**: 
  - `update`: O(log n) due to insertion and deletion in a multiset.
  - `current`: O(1)
  - `maximum`, `minimum`: O(1) for retrieval from a multiset.
- **Space Complexity**: O(n) for storing the stock price map and multiset.

Both approaches aim to maintain efficient operations for stock price updates and queries. The second approach optimizes the retrieval of maximum and minimum prices using a balanced data structure.

