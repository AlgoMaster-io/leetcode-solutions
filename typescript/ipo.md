# [Leetcode 502: IPO](https://leetcode.com/problems/ipo/)

## Table of Contents
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Optimized Approach using Max-Heap](#approach-2-optimized-approach-using-max-heap)

### Approach 1: Brute Force

**Intuition:**

The main idea is to select projects that can be afforded with the current available capital and give the maximum profit. A brute force way is to iterate through the list of projects and select the best possible one with the available capital.

For each round, we iterate over all projects to find the best one that can be completed, and add its profit to the capital.

**Steps:**
1. Initialize a variable for current capital `currentCapital = w`.
2. Repeat `k` times:
   - Find the most profitable project that is affordable with `currentCapital`.
   - Add that project’s profit to `currentCapital`.

**Time Complexity:** O(n * k), where `n` is the number of projects and `k` is the number of projects that can be selected.  
**Space Complexity:** O(1), since we are using a constant amount of additional space.

```typescript
function findMaximizedCapital(k: number, w: number, profits: number[], capital: number[]): number {
    let currentCapital = w;

    for (let i = 0; i < k; i++) {
        let maxProfitIndex = -1;
        for (let j = 0; j < profits.length; j++) {
            // Check if the project is affordable and more profitable.
            if (capital[j] <= currentCapital) {
                if (maxProfitIndex === -1 || profits[j] > profits[maxProfitIndex]) {
                    maxProfitIndex = j;
                }
            }
        }

        // If no project can be undertaken, break early.
        if (maxProfitIndex === -1) break;

        // Update the capital with the profit of chosen project.
        currentCapital += profits[maxProfitIndex];

        // Mark the chosen project as done by making it unaffordable.
        capital[maxProfitIndex] = Infinity;
    }

    return currentCapital;
}
```

### Approach 2: Optimized Approach using Max-Heap

**Intuition:**

We can optimize the above approach by using a max-heap (priority queue) to always fetch the most profitable project we can afford. This approach reduces the need to repeatedly search through the list.

**Steps:**
1. Pair each project with its capital required and sort these pairs by capital required.
2. Use a max-heap to keep track of projects' profits that can be undertaken with the current capital.
3. Repeat `k` times:
   - Push all affordable projects into the max-heap.
   - Pop the max profit project from the heap and add the profit to the capital.
4. Return the current capital after `k` choices.

**Time Complexity:** O(n log n + k log n), mainly due to the sorting and heap operations.  
**Space Complexity:** O(n), for storing the projects in the heap.

```typescript
function findMaximizedCapital(k: number, w: number, profits: number[], capital: number[]): number {
    const projects: [number, number][] = profits.map((profit, i) => [capital[i], profit]);

    // Sort projects based on capital required for starting them.
    projects.sort((a, b) => a[0] - b[0]);

    const maxHeap = new MaxHeap<number>();
    let currentCapital = w;
    let projectIndex = 0;

    for (let i = 0; i < k; i++) {
        // Add all projects that can be completed with current capital to the max-heap.
        while (projectIndex < projects.length && projects[projectIndex][0] <= currentCapital) {
            maxHeap.insert(projects[projectIndex][1]);
            projectIndex++;
        }

        // If there are no projects we can do, return early.
        if (maxHeap.isEmpty()) {
            break;
        }

        // Select the most profitable project we can afford and increase current capital.
        currentCapital += maxHeap.extractMax();
    }

    return currentCapital;
}

// A simple max-heap implementation.
class MaxHeap<T> {
    private heap: T[] = [];

    insert(value: T): void {
        this.heap.push(value);
        this.bubbleUp();
    }

    extractMax(): T {
        if (this.isEmpty()) throw new Error('Heap is empty');
        
        const max = this.heap[0];
        const end = this.heap.pop()!;
        
        if (this.heap.length > 0) {
            this.heap[0] = end;
            this.bubbleDown();
        }
        
        return max;
    }

    isEmpty(): boolean {
        return this.heap.length === 0;
    }

    private bubbleUp(): void {
        let index = this.heap.length - 1;
        const element = this.heap[index];

        while (index > 0) {
            const parentIndex = Math.floor((index - 1) / 2);
            const parent = this.heap[parentIndex];
            
            if (element <= parent) break;
            
            this.heap[index] = parent;
            index = parentIndex;
        }
        
        this.heap[index] = element;
    }

    private bubbleDown(): void {
        let index = 0;
        const length = this.heap.length;
        const element = this.heap[0];

        while (true) {
            const leftChildIndex = 2 * index + 1;
            const rightChildIndex = 2 * index + 2;
            let leftChild: T, rightChild: T;
            let swapIndex: number | null = null;

            if (leftChildIndex < length) {
                leftChild = this.heap[leftChildIndex];
                if (leftChild > element) {
                    swapIndex = leftChildIndex;
                }
            }

            if (rightChildIndex < length) {
                rightChild = this.heap[rightChildIndex];
                if (
                    (swapIndex === null && rightChild > element) ||
                    (swapIndex !== null && rightChild > leftChild!)
                ) {
                    swapIndex = rightChildIndex;
                }
            }

            if (swapIndex === null) break;
            
            this.heap[index] = this.heap[swapIndex];
            index = swapIndex;
        }

        this.heap[index] = element;
    }
}
```

In this solution, the use of a max-heap allows us to efficiently select the most profitable project the moment it can be funded with the available capital. By organizing projects based on the required capital and leveraging priority queues, we significantly improve upon the brute-force method.

