# [Leetcode 2034: Stock Price Fluctuation](https://leetcode.com/problems/stock-price-fluctuation)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Using Two Heaps with a HashMap](#approach-2-using-two-heaps-with-a-hashmap)
  
### Approach 1: Brute Force

#### Intuition:
The simplest approach is to use an unordered map (or object in the case of JavaScript/TypeScript) to store the timestamp and their corresponding stock prices. For getting the latest price, we need to iterate through the map to find the maximum timestamp.

However, this approach is not optimal for getting the maximum and minimum prices at any time, since it requires iterating through all entries.

#### Time Complexity:
- `update()`: O(1)
- `current()`: O(n), where n is the number of timestamps
- `maximum()`: O(n)
- `minimum()`: O(n)

#### Space Complexity:
- O(n), where n is the number of timestamps

```typescript
class StockPrice {
    private prices: Map<number, number>;
    
    constructor() {
        this.prices = new Map();
    }
    
    // Update the price at a given timestamp
    update(timestamp: number, price: number): void {
        this.prices.set(timestamp, price);
    }
    
    // Get the current (latest recorded) price
    current(): number {
        let maxTimestamp = -Infinity;
        for (const timestamp of this.prices.keys()) {
            maxTimestamp = Math.max(maxTimestamp, timestamp);
        }
        return this.prices.get(maxTimestamp)!;
    }
    
    // Get the maximum price across all timestamps
    maximum(): number {
        let maxPrice = -Infinity;
        for (const price of this.prices.values()) {
            maxPrice = Math.max(maxPrice, price);
        }
        return maxPrice;
    }
    
    // Get the minimum price across all timestamps
    minimum(): number {
        let minPrice = Infinity;
        for (const price of this.prices.values()) {
            minPrice = Math.min(minPrice, price);
        }
        return minPrice;
    }
}
```

### Approach 2: Using Two Heaps with a HashMap

#### Intuition:
By using two heaps (a max-heap and a min-heap) along with a hashmap, we can efficiently get the maximum and minimum stock prices. The hashmap will keep track of current prices at specific timestamps, while the heaps will help us retrieve the maximum and minimum efficiently.

To handle updates, we will ensure that any outdated prices (not the current one for a specific timestamp) in the heaps are lazy-deleted on access.

#### Time Complexity:
- `update()`: O(log n), where n is the number of timestamps in each heap operation
- `current()`: O(1)
- `maximum()`: O(log n)
- `minimum()`: O(log n)

#### Space Complexity:
- O(n), where n is the number of timestamps

```typescript
class MaxHeap {
    private heap: number[];

    constructor() {
        this.heap = [];
    }

    push(val: number): void {
        this.heap.push(val);
        this._heapifyUp(this.heap.length - 1);
    }

    pop(): number | undefined {
        if (this.heap.length === 0) return undefined;
        const max = this.heap[0];
        const end = this.heap.pop();
        if (end !== undefined && this.heap.length > 0) {
            this.heap[0] = end;
            this._heapifyDown(0);
        }
        return max;
    }

    peek(): number | undefined {
        return this.heap.length > 0 ? this.heap[0] : undefined;
    }

    private _heapifyUp(idx: number): void {
        let parent = Math.floor((idx - 1) / 2);
        while (idx > 0 && this.heap[idx] > this.heap[parent]) {
            [this.heap[idx], this.heap[parent]] = [this.heap[parent], this.heap[idx]];
            idx = parent;
            parent = Math.floor((idx - 1) / 2);
        }
    }

    private _heapifyDown(idx: number): void {
        const length = this.heap.length;
        let left = 2 * idx + 1;
        let right = 2 * idx + 2;
        let largest = idx;
        
        if (left < length && this.heap[left] > this.heap[largest]) {
            largest = left;
        }
        if (right < length && this.heap[right] > this.heap[largest]) {
            largest = right;
        }
        if (largest !== idx) {
            [this.heap[idx], this.heap[largest]] = [this.heap[largest], this.heap[idx]];
            this._heapifyDown(largest);
        }
    }
}

class MinHeap {
    private heap: number[];

    constructor() {
        this.heap = [];
    }

    push(val: number): void {
        this.heap.push(val);
        this._heapifyUp(this.heap.length - 1);
    }

    pop(): number | undefined {
        if (this.heap.length === 0) return undefined;
        const min = this.heap[0];
        const end = this.heap.pop();
        if (end !== undefined && this.heap.length > 0) {
            this.heap[0] = end;
            this._heapifyDown(0);
        }
        return min;
    }

    peek(): number | undefined {
        return this.heap.length > 0 ? this.heap[0] : undefined;
    }

    private _heapifyUp(idx: number): void {
        let parent = Math.floor((idx - 1) / 2);
        while (idx > 0 && this.heap[idx] < this.heap[parent]) {
            [this.heap[idx], this.heap[parent]] = [this.heap[parent], this.heap[idx]];
            idx = parent;
            parent = Math.floor((idx - 1) / 2);
        }
    }

    private _heapifyDown(idx: number): void {
        const length = this.heap.length;
        let left = 2 * idx + 1;
        let right = 2 * idx + 2;
        let smallest = idx;
        
        if (left < length && this.heap[left] < this.heap[smallest]) {
            smallest = left;
        }
        if (right < length && this.heap[right] < this.heap[smallest]) {
            smallest = right;
        }
        if (smallest !== idx) {
            [this.heap[idx], this.heap[smallest]] = [this.heap[smallest], this.heap[idx]];
            this._heapifyDown(smallest);
        }
    }
}

class StockPrice {
    private timestampPriceMap: Map<number, number>;
    private maxHeap: MaxHeap;
    private minHeap: MinHeap;
    private latestTimestamp: number;
    
    constructor() {
        this.timestampPriceMap = new Map();
        this.maxHeap = new MaxHeap();
        this.minHeap = new MinHeap();
        this.latestTimestamp = 0;
    }
    
    update(timestamp: number, price: number): void {
        this.timestampPriceMap.set(timestamp, price);
        this.maxHeap.push(price);
        this.minHeap.push(price);
        this.latestTimestamp = Math.max(this.latestTimestamp, timestamp);
    }
    
    current(): number {
        return this.timestampPriceMap.get(this.latestTimestamp)!;
    }
    
    maximum(): number {
        while (this.maxHeap.peek() !== undefined && this.maxHeap.peek() !== this.timestampPriceMap.get(Array.from(this.timestampPriceMap.keys()).find(key => this.timestampPriceMap.get(key) === this.maxHeap.peek())!)) {
            this.maxHeap.pop();
        }
        return this.maxHeap.peek()!;
    }
    
    minimum(): number {
        while (this.minHeap.peek() !== undefined && this.minHeap.peek() !== this.timestampPriceMap.get(Array.from(this.timestampPriceMap.keys()).find(key => this.timestampPriceMap.get(key) === this.minHeap.peek())!)) {
            this.minHeap.pop();
        }
        return this.minHeap.peek()!;
    }
}
```

Note: In practice, we would generally utilize existing heap implementations which handle duplicates more gracefully than shown here because the implementation here is rudimentary and for learning purposes. The logic should ensure heaps contain only valid current prices to optimize operations.

