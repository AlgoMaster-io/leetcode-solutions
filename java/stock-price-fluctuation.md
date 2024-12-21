# [Leetcode 2034: Stock Price Fluctuation](https://leetcode.com/problems/stock-price-fluctuation/)

## Approaches:
- [Approach 1: Brute Force with List](#approach-1-brute-force-with-list)
- [Approach 2: Using HashMap for Tracking Prices](#approach-2-using-hashmap-for-tracking-prices)
- [Approach 3: Using TreeMap for Efficient Ordering](#approach-3-using-treemap-for-efficient-ordering)

---

### Approach 1: Brute Force with List

#### Intuition:
The brute force approach is fairly straightforward. We maintain a list of pairs and simply update or query the list as required. This approach will straightforwardly tackle the problem but will have inefficiencies especially given the constraints.

#### Solution:
```java
import java.util.ArrayList;
import java.util.List;

class StockPrice {
    private List<int[]> prices;
    private int maxTimestamp;
    
    public StockPrice() {
        prices = new ArrayList<>();
        maxTimestamp = 0;
    }
    
    public void update(int timestamp, int price) {
        maxTimestamp = Math.max(maxTimestamp, timestamp);
        boolean found = false;
        for (int[] entry : prices) {
            if (entry[0] == timestamp) {
                entry[1] = price;
                found = true;
                break;
            }
        }
        if (!found) {
            prices.add(new int[]{timestamp, price});
        }
    }
    
    public int current() {
        for (int[] entry : prices) {
            if (entry[0] == maxTimestamp) {
                return entry[1];
            }
        }
        return -1; // This line should never be reached.
    }

    public int maximum() {
        int maxPrice = Integer.MIN_VALUE;
        for (int[] entry : prices) {
            maxPrice = Math.max(maxPrice, entry[1]);
        }
        return maxPrice;
    }

    public int minimum() {
        int minPrice = Integer.MAX_VALUE;
        for (int[] entry : prices) {
            minPrice = Math.min(minPrice, entry[1]);
        }
        return minPrice;
    }
}
```

#### Complexity Analysis:
- **Time Complexity**: 
  - `update`: O(n) where n is the number of prices, as we might need to traverse the entire list to update a price.
  - `current`: O(n) to find the current price matching the maximum timestamp.
  - `maximum` and `minimum`: O(n) each to find the maximum and minimum prices.
- **Space Complexity**: O(n) since we store up to n timestamps and prices.

---

### Approach 2: Using HashMap for Tracking Prices

#### Intuition:
Using a HashMap allows for more efficient updates. By keeping track of the prices with their timestamps in a map, the update and current operations become more efficient. However, we still need list-like structures to calculate max and min in a straightforward method or employ further structures to maintain these values efficiently.

#### Solution:
```java
import java.util.HashMap;

class StockPrice {
    private HashMap<Integer, Integer> priceMap;
    private int maxTimestamp;
    
    public StockPrice() {
        priceMap = new HashMap<>();
        maxTimestamp = 0;
    }
    
    public void update(int timestamp, int price) {
        priceMap.put(timestamp, price);
        maxTimestamp = Math.max(maxTimestamp, timestamp);
    }
    
    public int current() {
        return priceMap.get(maxTimestamp);
    }

    public int maximum() {
        int maxPrice = Integer.MIN_VALUE;
        for (int price : priceMap.values()) {
            maxPrice = Math.max(maxPrice, price);
        }
        return maxPrice;
    }

    public int minimum() {
        int minPrice = Integer.MAX_VALUE;
        for (int price : priceMap.values()) {
            minPrice = Math.min(minPrice, price);
        }
        return minPrice;
    }
}
```

#### Complexity Analysis:
- **Time Complexity**: 
  - `update` and `current`: O(1) due to HashMap operations.
  - `maximum` and `minimum`: O(n) because we still have to possibly traverse all prices.
- **Space Complexity**: O(n) for storing n entries in the map.

---

### Approach 3: Using TreeMap for Efficient Ordering

#### Intuition:
TreeMap inherently sorts its keys, providing a way to efficiently find min and max along with an easy way to get the current price using the highest timestamp. This allows all operations to be more efficient except adding or removing, which are logarithmic due to the properties of the TreeMap.

#### Solution:
```java
import java.util.Map;
import java.util.TreeMap;

class StockPrice {
    private TreeMap<Integer, Integer> prices;
    private TreeMap<Integer, Integer> count;
    private int maxTimestamp;
    
    public StockPrice() {
        prices = new TreeMap<>();
        count = new TreeMap<>();
        maxTimestamp = 0;
    }
    
    public void update(int timestamp, int price) {
        if (prices.containsKey(timestamp)) {
            int oldPrice = prices.get(timestamp);
            count.put(oldPrice, count.get(oldPrice) - 1);
            if (count.get(oldPrice) == 0) {
                count.remove(oldPrice);
            }
        }
        prices.put(timestamp, price);
        count.put(price, count.getOrDefault(price, 0) + 1);
        maxTimestamp = Math.max(maxTimestamp, timestamp);
    }
    
    public int current() {
        return prices.get(maxTimestamp);
    }

    public int maximum() {
        return count.lastKey();
    }

    public int minimum() {
        return count.firstKey();
    }
}
```

#### Complexity Analysis:
- **Time Complexity**: 
  - `update`: O(log n) for both adding the new timestamp-price pair and updating counting in the `count` map.
  - `current`: O(1) using TreeMap's floor function.
  - `maximum` and `minimum`: O(1) to directly access the max and min keys in the `count` TreeMap.
- **Space Complexity**: O(n) as we store prices and counts for up to n different timestamps.

This approach provides the most optimal balance between insertion and query operations, allowing for quick access to min, max, and current prices.

