# [2034. Stock Price Fluctuation](https://leetcode.com/problems/stock-price-fluctuation/)

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Approach using Two Heaps and Dictionary](#optimized-approach-using-two-heaps-and-dictionary)

---

## Brute Force Approach

### Intuition and Approach

The problem involves implementing a data structure that supports retrieving the latest stock price and both the maximum and minimum prices efficiently. A straightforward solution would be to store all stock prices in a list indexed by time. This allows easy updates and retrievals. However, the retrievals for the max and min prices will require a linear scan of the list, which can be inefficient as the number of updates grows.

For each operation:
- To update the stock price at a specific timestamp, store it directly in a dictionary as a form of registry or history.
- To obtain the current price, simply retrieve the value of the latest timestamp.
- To find the maximum or minimum price, iterate through all stored values and keep track of the highest and lowest respectively.

```python
class StockPrice:

    def __init__(self):
        # Dictionary to store prices by their timestamps
        self.timestamp_to_price = {}
        # Initialize the latest timestamp
        self.latest_timestamp = 0

    def update(self, timestamp: int, price: int):
        # Update the price for a given timestamp
        self.timestamp_to_price[timestamp] = price
        # Update the latest timestamp if the current timestamp is the latest
        self.latest_timestamp = max(self.latest_timestamp, timestamp)

    def current(self) -> int:
        # Return the current price at the latest timestamp
        return self.timestamp_to_price[self.latest_timestamp]

    def maximum(self) -> int:
        # Return the maximum price by iterating over all prices
        return max(self.timestamp_to_price.values())

    def minimum(self) -> int:
        # Return the minimum price by iterating over all prices
        return min(self.timestamp_to_price.values())
```

### Time Complexity

- `update`: \(O(1)\), as we're simply updating a dictionary.
- `current`: \(O(1)\), direct access to the latest timestamp.
- `maximum`/`minimum`: \(O(n)\), where \(n\) is the number of timestamps, since we have to iterate over all entries.

### Space Complexity

- \(O(n)\), where \(n\) is the number of timestamps stored in the dictionary.

---

## Optimized Approach using Two Heaps and Dictionary

### Intuition and Approach

The primary bottleneck in the brute force approach is efficiently finding the maximum and minimum prices. A more efficient strategy is to use two heaps (a max heap and a min heap) to dynamically track the highest and lowest stock prices.

The idea is:
- Use a dictionary to map timestamps to prices.
- Maintain a max heap for retrieving the maximum price and a min heap for the minimum price.
- The heaps allow for efficient retrievals, and each update operation is managed by keeping the heaps synchronized with the rest of the data.

```python
import heapq

class StockPrice:

    def __init__(self):
        # Dictionary to store prices by their timestamps
        self.timestamp_to_price = {}
        # Initialize the latest timestamp
        self.latest_timestamp = 0
        # Max-heap for maximum price and Min-heap for minimum price
        self.max_heap = []
        self.min_heap = []

    def update(self, timestamp: int, price: int):
        # Update the price for a given timestamp
        self.timestamp_to_price[timestamp] = price
        # Add entry to both heaps
        heapq.heappush(self.max_heap, (-price, timestamp))
        heapq.heappush(self.min_heap, (price, timestamp))
        # Update the latest timestamp if the current timestamp is the latest
        self.latest_timestamp = max(self.latest_timestamp, timestamp)

    def current(self) -> int:
        # Return the current price at the latest timestamp
        return self.timestamp_to_price[self.latest_timestamp]

    def maximum(self) -> int:
        # Remove invalid entries from max-heap
        while True:
            price, timestamp = self.max_heap[0]
            # If the top of the heap is valid, return it
            if self.timestamp_to_price[timestamp] == -price:
                return -price
            # Otherwise, discard it
            heapq.heappop(self.max_heap)

    def minimum(self) -> int:
        # Remove invalid entries from min-heap
        while True:
            price, timestamp = self.min_heap[0]
            # If the top of the heap is valid, return it
            if self.timestamp_to_price[timestamp] == price:
                return price
            # Otherwise, discard it
            heapq.heappop(self.min_heap)
```

### Time Complexity

- `update`: \(O(\log n)\), because we insert an item into both a max-heap and a min-heap.
- `current`: \(O(1)\), as it involves direct dictionary access.
- `maximum`/`minimum`: Amortized \(O(\log n)\), as the heap supports efficient insertions and deletion of max/min elements.

### Space Complexity

- \(O(n)\), due to the dictionary and heaps storing timestamps and prices.

