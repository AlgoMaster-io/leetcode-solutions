# [Leetcode 871: Minimum Number of Refueling Stops](https://leetcode.com/problems/minimum-number-of-refueling-stops/)

## Approaches
1. [Dynamic Programming Approach](#dynamic-programming-approach)
2. [Greedy Approach Using Max Heap](#greedy-approach-using-max-heap)

---

## Dynamic Programming Approach

### Solution Intuition:
The dynamic programming approach involves breaking down the problem into subproblems and solving them iteratively. Here, the idea is to use a dp array where `dp[i]` represents the maximum distance that can be reached with `i` refueling stops. We initialize `dp[0]` with the initial fuel. For each station, we iterate backward to update the dp array to reflect the additional distance achievable by using the fuel available at that station.

### Steps:
1. Initialize a dp array with a size `n + 1`, where `n` is the number of fuel stations. Set `dp[0] = startFuel`.
2. For each station, update the dp array from right to left. This ensures that we use each station only once.
3. After processing all stations, find the smallest index `i` such that `dp[i] >= target`. If no such index exists, return -1.

### Time and Space Complexity:
- **Time Complexity:** O(n^2), where n is the number of stations. Each station potentially updates the distances of up to n+1 entries in the dp array.
- **Space Complexity:** O(n), for the dp array.

### Code:
```javascript
function minRefuelStops(target, startFuel, stations) {
    const n = stations.length;
    const dp = Array(n + 1).fill(0);
    dp[0] = startFuel;
    
    for (let i = 0; i < n; ++i) {
        for (let t = i; t >= 0; --t) {
            // Check if current dp[t] can reach the station at i
            if (dp[t] >= stations[i][0]) {
                dp[t + 1] = Math.max(dp[t + 1], dp[t] + stations[i][1]);
            }
        }
    }
    
    for (let i = 0; i <= n; ++i) {
        if (dp[i] >= target) return i;
    }
    return -1;
}
```

---

## Greedy Approach Using Max Heap

### Solution Intuition:
The greedy approach involves using a max heap (priority queue) to always refuel with the largest available fuel from the past stations whenever needed. You traverse through the stations, keeping track of fuel and distance, and each time you can't reach the next station, you refuel from the largest available fuel in the max heap.

### Steps:
1. Use a priority queue (max heap) to keep track of fuel from stations we have passed.
2. Iterate through the path until reaching the target.
3. Each time the current fuel is insufficient to reach the next station, refuel from the max heap until you can reach the next position or the heap is empty.
4. If the heap is empty and you cannot reach the next station or the target, return -1.

### Time and Space Complexity:
- **Time Complexity:** O(n log n), as every fuel addition to the max heap and extraction from the heap takes O(log n) time.
- **Space Complexity:** O(n), for storing fuel in the max heap.

### Code:
```javascript
function minRefuelStops(target, startFuel, stations) {
    let fuel = startFuel, position = 0, refuels = 0;
    const maxHeap = new MaxHeap();
    
    stations.push([target, 0]); // Treat target as a station for simplifying logic
    
    for (let i = 0; i <= stations.length; i++) {
        while (fuel < stations[i][0]) {
            if (maxHeap.isEmpty()) return -1;
            fuel += maxHeap.extractMax();
            refuels++;
        }
        if (i < stations.length) {
            maxHeap.insert(stations[i][1]);
        }
    }
    return refuels;
}

// MaxHeap implementation using an array
class MaxHeap {
    constructor() {
        this.heap = [];
    }

    insert(val) {
        this.heap.push(val);
        this._siftUp(this.heap.length - 1);
    }

    extractMax() {
        if (this.heap.length === 1) return this.heap.pop();
        const max = this.heap[0];
        this.heap[0] = this.heap.pop();
        this._siftDown(0);
        return max;
    }

    isEmpty() {
        return this.heap.length === 0;
    }

    _siftUp(index) {
        while (index > 0) {
            const parent = Math.floor((index - 1) / 2);
            if (this.heap[parent] >= this.heap[index]) break;
            [this.heap[parent], this.heap[index]] = [this.heap[index], this.heap[parent]];
            index = parent;
        }
    }

    _siftDown(index) {
        const lastIndex = this.heap.length - 1;
        while (index * 2 + 1 <= lastIndex) {
            let largest = index;
            const leftChild = index * 2 + 1;
            const rightChild = index * 2 + 2;

            if (this.heap[leftChild] > this.heap[largest]) largest = leftChild;
            if (rightChild <= lastIndex && this.heap[rightChild] > this.heap[largest]) largest = rightChild;

            if (largest === index) break;
            [this.heap[index], this.heap[largest]] = [this.heap[largest], this.heap[index]];
            index = largest;
        }
    }
}
```

This provides a basic dynamic programming and a more efficient greedy solution using max heap to the problem of finding the minimum number of refueling stops required to reach the target.


