[Leetcode Problem 2034: Stock Price Fluctuation](https://leetcode.com/problems/stock-price-fluctuation/)

## Approaches:
- [Approach 1: Brute Force using Arrays](#approach-1-brute-force-using-arrays)
- [Approach 2: Optimized using HashMap and Sorted Structures](#approach-2-optimized-using-hashmap-and-sorted-structures)

### Approach 1: Brute Force using Arrays

**Intuition:**

In a brute force approach, we can use simple arrays to keep track of all the `(timestamp, price)` pairs received. At any point in time, the current price is the value of the last element in the array. To find minimum and maximum prices, we iterate over the entire list of prices.

**Solution Code:**

```javascript
class StockPrice {
    constructor() {
        this.priceList = []; // List to store (timestamp, price) pairs
    }
    
    update(timestamp, price) {
        // Append the new price entry to the list
        this.priceList.push([timestamp, price]);
    }
    
    current() {
        // Return the price at the last timestamp
        return this.priceList[this.priceList.length - 1][1];
    }
    
    maximum() {
        // Find the maximum price by traversing the list
        let maxPrice = 0;
        for (let i = 0; i < this.priceList.length; i++) {
            maxPrice = Math.max(maxPrice, this.priceList[i][1]);
        }
        return maxPrice;
    }
    
    minimum() {
        // Find the minimum price by traversing the list
        let minPrice = Infinity;
        for (let i = 0; i < this.priceList.length; i++) {
            minPrice = Math.min(minPrice, this.priceList[i][1]);
        }
        return minPrice;
    }
}
```

**Time Complexity:**
- `update()`: O(1), as we are simply appending the new price to the list.
- `current()`: O(1), as we access the last element.
- `maximum()` / `minimum()`: O(n), as we need to traverse the entire array.

**Space Complexity:**
- O(n), where n is the number of prices stored.

### Approach 2: Optimized using HashMap and Sorted Structures

**Intuition:**

To optimize the operation of finding maximum and minimum prices, we can leverage a HashMap to store the latest prices for each timestamp and use separate data structures such as a Sorted List or two Heaps (one for min and one for max) to efficiently retrieve the current maximum and minimum.

**Solution Code:**

```javascript
class StockPrice {
    constructor() {
        this.timestampPriceMap = new Map(); // To store the price for each timestamp
        this.maxTimestamp = 0; // To track the latest timestamp
        this.pricesFrequency = new Map(); // To store frequency of each price
        this.maxHeap = []; // Max-Heap for max prices
        this.minHeap = []; // Min-Heap for min prices
    }
    
    update(timestamp, price) {
        // Update price for the timestamp
        if (this.timestampPriceMap.has(timestamp)) {
            // Remove the frequency of the old price
            const oldPrice = this.timestampPriceMap.get(timestamp);
            this.pricesFrequency.set(oldPrice, this.pricesFrequency.get(oldPrice) - 1);
            if (this.pricesFrequency.get(oldPrice) === 0) {
                this.pricesFrequency.delete(oldPrice);
            }
        }
        
        this.timestampPriceMap.set(timestamp, price);
        this.maxTimestamp = Math.max(this.maxTimestamp, timestamp);
        
        // Update frequency of the new price
        if (!this.pricesFrequency.has(price)) {
            this.pricesFrequency.set(price, 0);
        }
        this.pricesFrequency.set(price, this.pricesFrequency.get(price) + 1);
        
        // Push the new price to heaps
        this.maxHeap.push(-price);
        this.minHeap.push(price);
    }
    
    current() {
        return this.timestampPriceMap.get(this.maxTimestamp);
    }
    
    maximum() {
        // Ensure top of max heap reflects a valid price
        while (this.pricesFrequency.get(-this.maxHeap[0]) === undefined) {
            this.maxHeap.shift();
        }
        return -this.maxHeap[0];
    }
    
    minimum() {
        // Ensure top of min heap reflects a valid price
        while (this.pricesFrequency.get(this.minHeap[0]) === undefined) {
            this.minHeap.shift();
        }
        return this.minHeap[0];
    }
}
```

**Time Complexity:**
- `update()`: O(log n) due to heap operations.
- `current()`: O(1), hashmap access.
- `maximum()` / `minimum()`: O(log n), potentially requires heap rebalance.

**Space Complexity:**
- O(n), where n is the number of updates. 

This solution optimizes price retrieval operations by utilizing efficient data structures to keep track of the max and min prices with amortized logarithmic complexity.

