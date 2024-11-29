# [1696. Jump Game VI](https://leetcode.com/problems/jump-game-vi/)

## Approach 1: Dynamic Programming with Sliding Window (Deque)

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
function maxResult(nums: number[], k: number): number {
    const deque: number[] = []; // Stores indices of potential max scores
    deque.push(0);
    const dp: number[] = new Array(nums.length).fill(0);
    dp[0] = nums[0];

    for (let i = 1; i < nums.length; i++) {
        // Remove indices that are out of the current window
        if (deque.length > 0 && deque[0] < i - k) {
            deque.shift();
        }

        // Update dp[i] using the max value in the deque
        dp[i] = nums[i] + dp[deque[0]];

        // Maintain the deque to keep indices in decreasing order of their dp values
        while (deque.length > 0 && dp[deque[deque.length - 1]] <= dp[i]) {
            deque.pop();
        }

        deque.push(i);
    }

    return dp[nums.length - 1];
}
```

## Approach 2: Priority Queue (Heap-Based)

### Solution
typescript
```typescript
// Time Complexity: O(n log k)
// Space Complexity: O(k)
function maxResult(nums: number[], k: number): number {
    const maxHeap = new MaxHeap();  // Max-heap (custom implementation)
    maxHeap.offer([nums[0], 0]);    // Store [score, index]
    let score = nums[0];

    for (let i = 1; i < nums.length; i++) {
        // Remove elements outside the current window
        while (maxHeap.peek()[1] < i - k) {
            maxHeap.poll();
        }

        // The current score is the sum of the max score in the heap and nums[i]
        score = nums[i] + maxHeap.peek()[0];
        maxHeap.offer([score, i]);
    }

    return score;
}

// Helper class: Custom Max Heap implementation
class MaxHeap {
    private heap: number[][];

    constructor() {
        this.heap = [];
    }

    offer(val: number[]): void {
        this.heap.push(val);
        this.bubbleUp(this.heap.length - 1);
    }

    poll(): number[] {
        const top = this.peek();
        const end = this.heap.pop();
        if (this.heap.length > 0 && end) {
            this.heap[0] = end;
            this.bubbleDown(0);
        }
        return top;
    }

    peek(): number[] {
        return this.heap[0];
    }

    private bubbleUp(index: number) {
        const element = this.heap[index];
        while (index > 0) {
            const parentIndex = Math.floor((index - 1) / 2);
            const parent = this.heap[parentIndex];
            
            if (element[0] <= parent[0]) break;

            this.heap[parentIndex] = element;
            this.heap[index] = parent;
            index = parentIndex;
        }
    }

    private bubbleDown(index: number) {
        const length = this.heap.length;
        const element = this.heap[index];
        while (true) {
            let leftChildIndex = 2 * index + 1;
            let rightChildIndex = 2 * index + 2;
            let leftChild, rightChild;
            let swap = null;

            if (leftChildIndex < length) {
                leftChild = this.heap[leftChildIndex];
                if (leftChild[0] > element[0]) {
                    swap = leftChildIndex;
                }
            }

            if (rightChildIndex < length) {
                rightChild = this.heap[rightChildIndex];
                if (
                    (swap === null && rightChild[0] > element[0]) || 
                    (swap !== null && rightChild[0] > leftChild[0])
                ) {
                    swap = rightChildIndex;
                }
            }

            if (swap === null) break;
            this.heap[index] = this.heap[swap];
            this.heap[swap] = element;
            index = swap;
        }
    }
}
```

