## [Leetcode 2034: Stock Price Fluctuation](https://leetcode.com/problems/stock-price-fluctuation)

### Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Optimal Approach using HashMap and Sorted Containers](#optimal-approach-using-hashmap-and-sorted-containers)

---

### Brute Force Approach

#### Intuition
The brute force approach involves storing all the price entries (timestamps and prices) and performing operations directly on these lists. This approach is straightforward but not efficient for large inputs, particularly when frequent price updates and queries are expected.

#### Implementation

```go
type StockPrice struct {
    prices    map[int]int
    timestamps []int
}

func Constructor() StockPrice {
    return StockPrice{
        prices:    make(map[int]int),
        timestamps: []int{},
    }
}

func (this *StockPrice) Update(timestamp int, price int)  {
    if _, exists := this.prices[timestamp]; !exists {
        this.timestamps = append(this.timestamps, timestamp)
    }
    this.prices[timestamp] = price
}

func (this *StockPrice) Current() int {
    latestTimestamp := this.timestamps[len(this.timestamps)-1]
    return this.prices[latestTimestamp]
}

func (this *StockPrice) Maximum() int {
    maxPrice := 0
    for _, price := range this.prices {
        if price > maxPrice {
            maxPrice = price
        }
    }
    return maxPrice
}

func (this *StockPrice) Minimum() int {
    minPrice := int(^uint(0) >> 1) // Max value of int
    for _, price := range this.prices {
        if price < minPrice {
            minPrice = price
        }
    }
    return minPrice
}
```

#### Complexity Analysis
- **Time Complexity**: 
  - `Update()`, `Current()` operations are O(1).
  - `Maximum()`, `Minimum()` may take O(n) time.
- **Space Complexity**: O(n), for storing all timestamps and prices.

---

### Optimal Approach using HashMap and Sorted Containers

#### Intuition
For an optimal approach, we can separate concerns into tracking the latest price and efficiently managing the distribution of prices with regard to the number of times each price appears. For this, we can use a hashmap to keep track of time and price pairings, and two heaps or a balanced tree structure to quickly get the maximum and minimum prices.

#### Implementation

```go
type StockPrice struct {
    priceMap     map[int]int
    currentTime  int
    maxPriceHeap *IntMaxHeap
    minPriceHeap *IntMinHeap
}

func Constructor() StockPrice {
    return StockPrice{
        priceMap:     make(map[int]int),
        currentTime:  0,
        maxPriceHeap: &IntMaxHeap{},
        minPriceHeap: &IntMinHeap{},
    }
}

func (this *StockPrice) Update(timestamp int, price int) {
    this.currentTime = max(this.currentTime, timestamp)
    if prevPrice, exists := this.priceMap[timestamp]; exists {
        this.maxPriceHeap.Remove(prevPrice)
        this.minPriceHeap.Remove(prevPrice)
    }
    this.priceMap[timestamp] = price
    this.maxPriceHeap.Insert(price)
    this.minPriceHeap.Insert(price)
}

func (this *StockPrice) Current() int {
    return this.priceMap[this.currentTime]
}

func (this *StockPrice) Maximum() int {
    return this.maxPriceHeap.Max()
}

func (this *StockPrice) Minimum() int {
    return this.minPriceHeap.Min()
}

// Helper functions for the heap operations are assumed to be implemented
```

#### Complexity Analysis
- **Time Complexity**: 
  - `Update()`, `Current()`, `Maximum()`, `Minimum()` operations are O(log n) due to heap operations.
- **Space Complexity**: O(n), for storing all timestamps and prices.

This solution efficiently addresses the high frequency and demand of operations expected in real-world usage scenarios. It maintains optimal retrieval times for maximum, minimum, and current price using heaps for price management and hashmap for timestamp-price association.

