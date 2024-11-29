# 2034. [Stock Price Fluctuation](https://leetcode.com/problems/stock-price-fluctuation/)

## Approach: HashMap and Two Heaps

### Solution
go
```go
// Time Complexity:
//   - Update(): O(log n)
//   - Current(): O(1)
//   - Maximum(): O(1)
//   - Minimum(): O(1)
// Space Complexity: O(n), where n is the number of unique timestamps
package main

import "container/heap"

type StockPrice struct {
	latestTimestamp  int                       // Keeps track of the latest timestamp
	timestampPriceMap map[int]int             // Maps timestamp to price
	maxHeap           *MaxHeap                // To track maximum price
	minHeap           *MinHeap                // To track minimum price
	priceFrequencyMap map[int]int             // Maps each price to its frequency
}

func Constructor() StockPrice {
	return StockPrice{
		latestTimestamp: 0,
		timestampPriceMap: make(map[int]int),
		maxHeap: &MaxHeap{},
		minHeap: &MinHeap{},
		priceFrequencyMap: make(map[int]int),
	}
}

// Update the stock price at a given timestamp
func (sp *StockPrice) Update(timestamp int, price int) {
	// Update the latest timestamp if the current one is newer
	if timestamp > sp.latestTimestamp {
		sp.latestTimestamp = timestamp
	}

	// Adjust the frequency of the old price if timestamp exists
	if oldPrice, exists := sp.timestampPriceMap[timestamp]; exists {
		sp.priceFrequencyMap[oldPrice]--
		if sp.priceFrequencyMap[oldPrice] == 0 {
			delete(sp.priceFrequencyMap, oldPrice)
		}
	}

	// Update the map with the new price
	sp.timestampPriceMap[timestamp] = price
	sp.priceFrequencyMap[price]++

	// Add new price to both heaps and balances them
	heap.Push(sp.maxHeap, price)
	heap.Push(sp.minHeap, price)
}

// Return the latest price
func (sp *StockPrice) Current() int {
	return sp.timestampPriceMap[sp.latestTimestamp]
}

// Return the maximum price
func (sp *StockPrice) Maximum() int {
	// Remove outdated max prices
	for {
		if count, exists := sp.priceFrequencyMap[sp.maxHeap.Peek().(int)]; exists && count > 0 {
			break
		}
		heap.Pop(sp.maxHeap)
	}
	return sp.maxHeap.Peek().(int)
}

// Return the minimum price
func (sp *StockPrice) Minimum() int {
	// Remove outdated min prices
	for {
		if count, exists := sp.priceFrequencyMap[sp.minHeap.Peek().(int)]; exists && count > 0 {
			break
		}
		heap.Pop(sp.minHeap)
	}
	return sp.minHeap.Peek().(int)
}

// MaxHeap for maintaining maximum price
type MaxHeap []int

func (h MaxHeap) Len() int            { return len(h) }
func (h MaxHeap) Less(i, j int) bool  { return h[i] > h[j] }
func (h MaxHeap) Swap(i, j int)       { h[i], h[j] = h[j], h[i] }
func (h *MaxHeap) Push(x interface{}) { *h = append(*h, x.(int)) }
func (h *MaxHeap) Pop() interface{} {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[0 : n-1]
	return x
}
func (h MaxHeap) Peek() interface{} { return h[0] }

// MinHeap for maintaining minimum price
type MinHeap []int

func (h MinHeap) Len() int            { return len(h) }
func (h MinHeap) Less(i, j int) bool  { return h[i] < h[j] }
func (h MinHeap) Swap(i, j int)       { h[i], h[j] = h[j], h[i] }
func (h *MinHeap) Push(x interface{}) { *h = append(*h, x.(int)) }
func (h *MinHeap) Pop() interface{} {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[0 : n-1]
	return x
}
func (h MinHeap) Peek() interface{} { return h[0] }
```

