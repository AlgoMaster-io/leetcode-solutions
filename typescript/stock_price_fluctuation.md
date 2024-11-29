# 2034. [Stock Price Fluctuation](https://leetcode.com/problems/stock-price-fluctuation/)

## Approach: HashMap and Two Heaps

### Solution
typescript
```typescript
// Time Complexity:
//   - update(): O(log n)
//   - current(): O(1)
//   - maximum(): O(1)
//   - minimum(): O(1)
// Space Complexity: O(n), where n is the number of unique timestamps

class StockPrice {
    private latestTimestamp: number;
    private timestampPriceMap: Map<number, number>;
    private priceFrequencyMap: Map<number, number>;

    constructor() {
        this.latestTimestamp = 0;
        this.timestampPriceMap = new Map<number, number>();
        this.priceFrequencyMap = new Map<number, number>();
    }

    // Update the stock price at a given timestamp
    update(timestamp: number, price: number): void {
        this.latestTimestamp = Math.max(this.latestTimestamp, timestamp);

        // If the timestamp already exists, adjust the price frequency
        if (this.timestampPriceMap.has(timestamp)) {
            const oldPrice = this.timestampPriceMap.get(timestamp)!;
            this.priceFrequencyMap.set(oldPrice, this.priceFrequencyMap.get(oldPrice)! - 1);
            if (this.priceFrequencyMap.get(oldPrice) === 0) {
                this.priceFrequencyMap.delete(oldPrice);
            }
        }

        // Update the price for the timestamp
        this.timestampPriceMap.set(timestamp, price);
        this.priceFrequencyMap.set(price, (this.priceFrequencyMap.get(price) || 0) + 1);
    }

    // Return the latest price
    current(): number {
        return this.timestampPriceMap.get(this.latestTimestamp)!;
    }

    // Return the maximum price
    maximum(): number {
        return Math.max(...this.priceFrequencyMap.keys());
    }

    // Return the minimum price
    minimum(): number {
        return Math.min(...this.priceFrequencyMap.keys());
    }
}
```

