# [Leetcode 857: Minimum Cost to Hire K Workers](https://leetcode.com/problems/minimum-cost-to-hire-k-workers/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Sort by Ratio and Use Priority Queue](#approach-2-sort-by-ratio-and-use-priority-queue)

---

## Approach 1: Brute Force

### Intuition
In this approach, the idea is to calculate the possible costs of hiring every possible combination of k workers and keep track of the minimum cost found. This is the brute force solution where we explore all combinations.

### Detailed Explanation
1. **Generate All Combinations:** First, we generate all combinations of `k` workers from the list of workers.
2. **Calculate Cost for Each Combination:** For each possible combination, calculate the cost by determining the minimum wage needed for the combination to ensure fairness.
3. **Update Minimum Cost:** Compare the calculated cost with the current minimum cost and update if the new cost is less.

The core problem with this approach is its inefficiency for large inputs due to the exponential search space of combinations.

### Code
```javascript
// Not efficient for large input sizes
// Use this only for understanding or very small n values

function mincostToHireWorkers(quality, wage, K) {
    let n = quality.length;
    let minCost = Infinity;

    // Helper function to calculate cost
    function calculateCost(permutation) {
        let totalQuality = 0;
        let minRatio = -1;

        for (let idx of permutation) {
            totalQuality += quality[idx];
            minRatio = Math.max(minRatio, wage[idx] / quality[idx]);
        }

        return totalQuality * minRatio;
    }

    // Generate all combinations of K workers
    function combinations(arr, length) {
        function combineHelper(start, depth, combi) {
            if (depth === 0) {
                let cost = calculateCost(combi);
                if (cost < minCost) {
                    minCost = cost;
                }
                return;
            }
            for (let i = start; i <= arr.length - depth; i++) {
                combi.push(arr[i]);
                combineHelper(i + 1, depth - 1, combi);
                combi.pop();
            }
        }
        combineHelper(0, length, []);
    }

    combinations([...Array(n).keys()], K);

    return minCost;
}
```

### Complexity
- **Time Complexity:** \(O(\binom{n}{K} \times K)\)
- **Space Complexity:** \(O(K)\)

---

## Approach 2: Sort by Ratio and Use Priority Queue

### Intuition
To optimize, the key observation is that the total wage is determined by the worker with the maximum wage-to-quality ratio in any valid subset of workers. By sorting workers by this ratio, we can efficiently determine the minimum cost to hire `K` workers using a priority queue (or a max-heap).

### Detailed Explanation
1. **Calculate Ratio and Sort:** First, compute the ratio (`wage[i] / quality[i]`) for each worker and sort the workers by this ratio.
2. **Use Max-Heap for Quality:** Use a priority queue (max-heap) to keep track of the smallest `K` qualities while iterating through sorted workers.
3. **Iterate and Calculate Cost:** Start iterating through the sorted list, add the quality to the heap (priority queue). If the size exceeds `K`, remove the largest quality to ensure only the smallest `K` are considered. Calculate potential cost using the current worker's ratio and update the minimum cost.

### Code
```javascript
function mincostToHireWorkers(quality, wage, K) {
    // Combine quality and wage ratio, then sort by ratio
    let workers = quality.map((q, idx) => [q, wage[idx] / q]);
    workers.sort((a, b) => a[1] - b[1]);

    let qualitySum = 0;
    let maxHeap = new Heap((a, b) => b - a); // Max-heap as priority queue
    let minCost = Infinity;

    for (let [q, ratio] of workers) {
        maxHeap.push(q);
        qualitySum += q;

        // If we have more than K workers, remove the one with the largest quality
        if (maxHeap.size() > K) {
            qualitySum -= maxHeap.pop();
        }

        // If we have exactly K workers, calculate the cost
        if (maxHeap.size() === K) {
            minCost = Math.min(minCost, qualitySum * ratio);
        }
    }

    return minCost;
}

// Simple Max-Heap/Min-Heap class for demonstration
class Heap {
    constructor(compare) {
        this.compare = compare;
        this.data = [];
    }

    push(val) {
        this.data.push(val);
        this.bubbleUp();
    }

    pop() {
        if (this.data.length > 1) {
            this.swap(0, this.data.length - 1);
        }
        const poppedValue = this.data.pop();
        this.bubbleDown();
        return poppedValue;
    }

    size() {
        return this.data.length;
    }

    bubbleUp() {
        let index = this.data.length - 1;
        while (index > 0) {
            let element = this.data[index];
            let parentIndex = Math.floor((index - 1) / 2);
            let parent = this.data[parentIndex];

            if (this.compare(element, parent) <= 0) break;

            this.data[index] = parent;
            this.data[parentIndex] = element;
            index = parentIndex;
        }
    }

    bubbleDown() {
        let index = 0;
        const length = this.data.length;
        const element = this.data[0];

        while (true) {
            let leftChildIdx = 2 * index + 1;
            let rightChildIdx = 2 * index + 2;
            let leftChild, rightChild;
            let swap = null;

            if (leftChildIdx < length) {
                leftChild = this.data[leftChildIdx];
                if (this.compare(leftChild, element) > 0) {
                    swap = leftChildIdx;
                }
            }

            if (rightChildIdx < length) {
                rightChild = this.data[rightChildIdx];
                if (
                    (swap === null && this.compare(rightChild, element) > 0) ||
                    (swap !== null && this.compare(rightChild, leftChild) > 0)
                ) {
                    swap = rightChildIdx;
                }
            }

            if (swap === null) break;
            this.data[index] = this.data[swap];
            this.data[swap] = element;
            index = swap;
        }
    }

    swap(i, j) {
        [this.data[i], this.data[j]] = [this.data[j], this.data[i]];
    }
}
```

### Complexity
- **Time Complexity:** \(O(n \log n + n \log K)\) due to sorting and heap operations.
- **Space Complexity:** \(O(n + K)\) for storing worker data and heap.

