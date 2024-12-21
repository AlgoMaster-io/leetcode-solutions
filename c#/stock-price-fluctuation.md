# [Leetcode Problem 2034: Stock Price Fluctuation](https://leetcode.com/problems/stock-price-fluctuation/)

<sub>Approaches:</sub>
- [Approach 1: Using a Sorted Dictionary and a Priority Queue (MinHeap and MaxHeap)](#approach-1-using-a-sorted-dictionary-and-a-priority-queue-minheap-and-maxheap)

## Approach 1: Using a Sorted Dictionary and a Priority Queue (MinHeap and MaxHeap)

### Intuition

To implement a stock price monitoring system that allows us to update prices efficiently and retrieve the latest, minimum, and maximum prices, we can take advantage of several data structures:

1. **Sorted Dictionary**:
   - A SortedDictionary stores timestamps and their respective prices. It maintains keys in sorted order which helps to quickly determine the latest stock price by considering the last entry.

2. **Priority Queues (MinHeap and MaxHeap)**:
   - Use a MinHeap to always retrieve the minimum price efficiently.
   - Use a MaxHeap to retrieve the maximum price efficiently.

Here's a breakdown of the operations and how the data structures help achieve them:

- `update(timestamp, price)`:
  - Update the SortedDictionary with the timestamp and price. If the timestamp already exists, update the price to the latest one.

- `current()`:
  - Utilize the SortedDictionary to fetch the latest record. This is efficient since SortedDictionary provides direct access to both keys and values.

- `maximum()`:
  - Use a MaxHeap to track the maximum price. Since heaps automatically adjust and keep track of the extremum, the top element of the MaxHeap gives the maximum value efficiently.

- `minimum()`:
  - Use a MinHeap to track the minimum price in a similar way to the maximum.

### Time Complexity

- `update()`: O(log n), due to the insertion in the Sorted Dictionary.
- `current()`: O(1), as we directly access the last element in the dictionary.
- `maximum() / minimum()`: O(log n), due to the retrieval of the min/max element from the heaps.

### Space Complexity

- O(n), where n is the number of stock updates, due to storage in the Sorted Dictionary and heaps.

```csharp
using System;
using System.Collections.Generic;

public class StockPrice {
    private SortedDictionary<int, int> timestampPriceMap;
    private PriorityQueue<(int price, int timestamp), int> minHeap;
    private PriorityQueue<(int price, int timestamp), int> maxHeap;
    private int latestTimestamp;

    public StockPrice() {
        timestampPriceMap = new SortedDictionary<int, int>();
        minHeap = new PriorityQueue<(int, int), int>(Comparer<int>.Default);
        maxHeap = new PriorityQueue<(int, int), int>(Comparer<int>.Create((a, b) => b.CompareTo(a)));
        latestTimestamp = 0;
    }
    
    public void Update(int timestamp, int price) {
        latestTimestamp = Math.Max(latestTimestamp, timestamp);
        
        if (timestampPriceMap.ContainsKey(timestamp)) {
            int oldPrice = timestampPriceMap[timestamp];
            timestampPriceMap[timestamp] = price;

            // Add new prices regardless of previous state; lak of direct decrease in priority queue
            minHeap.Enqueue((price, timestamp), price);
            maxHeap.Enqueue((price, timestamp), price);
        } else {
            timestampPriceMap.Add(timestamp, price);
            minHeap.Enqueue((price, timestamp), price);
            maxHeap.Enqueue((price, timestamp), price);
        }
    }
    
    public int Current() {
        return timestampPriceMap[latestTimestamp];
    }
    
    public int Maximum() {
        // Remove stale elements
        while (timestampPriceMap[maxHeap.Peek().timestamp] != maxHeap.Peek().price) {
            maxHeap.Dequeue();
        }
        return maxHeap.Peek().price;
    }
    
    public int Minimum() {
        // Remove stale elements
        while (timestampPriceMap[minHeap.Peek().timestamp] != minHeap.Peek().price) {
            minHeap.Dequeue();
        }
        return minHeap.Peek().price;
    }
}
```

This approach balances time complexity for different operations, leveraging specific data structures to efficiently track and retrieve stock price updates.

