# 2034. [Stock Price Fluctuation](https://leetcode.com/problems/stock-price-fluctuation/)

## Approach: HashMap and Two Heaps

### Solution
python
```python
# Time Complexity:
#   - update(): O(log n)
#   - current(): O(1)
#   - maximum(): O(1)
#   - minimum(): O(1)
# Space Complexity: O(n), where n is the number of unique timestamps
from collections import defaultdict
import sortedcontainers

class StockPrice:

    def __init__(self):
        self.latest_timestamp = 0
        self.timestamp_price_map = {}
        self.price_frequency_map = sortedcontainers.SortedDict()

    # Update the stock price at a given timestamp
    def update(self, timestamp: int, price: int) -> None:
        self.latest_timestamp = max(self.latest_timestamp, timestamp)

        # If the timestamp already exists, adjust the price frequency
        if timestamp in self.timestamp_price_map:
            old_price = self.timestamp_price_map[timestamp]
            self.price_frequency_map[old_price] -= 1
            if self.price_frequency_map[old_price] == 0:
                del self.price_frequency_map[old_price]

        # Update the price for the timestamp
        self.timestamp_price_map[timestamp] = price
        if price in self.price_frequency_map:
            self.price_frequency_map[price] += 1
        else:
            self.price_frequency_map[price] = 1

    # Return the latest price
    def current(self) -> int:
        return self.timestamp_price_map[self.latest_timestamp]

    # Return the maximum price
    def maximum(self) -> int:
        return self.price_frequency_map.peekitem(-1)[0]

    # Return the minimum price
    def minimum(self) -> int:
        return self.price_frequency_map.peekitem(0)[0]
```

