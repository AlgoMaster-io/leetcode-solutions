# [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

## Approach 1: Using Deque (Optimal Solution)

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(k)
export class Solution {
    maxSlidingWindow(nums: number[], k: number): number[] {
        const n = nums.length;
        const result: number[] = new Array(n - k + 1);
        const deque: number[] = []; // Stores indices of elements

        for (let i = 0; i < n; i++) {
            // Remove indices out of the current window
            if (deque.length > 0 && deque[0] < i - k + 1) {
                deque.shift();
            }

            // Remove elements smaller than the current element from the back
            while (deque.length > 0 && nums[deque[deque.length - 1]] < nums[i]) {
                deque.pop();
            }

            deque.push(i); // Add the current element's index to the deque

            // Add the maximum element of the current window to the result
            if (i >= k - 1) {
                result[i - k + 1] = nums[deque[0]];
            }
        }

        return result;
    }
}
```

## Approach 2: Using Priority Queue (Heap-Based)

### Solution
typescript
```typescript
// Time Complexity: O(n log k)
// Space Complexity: O(k)

class MaxHeap {
    private heap: number[][]

    constructor() {
        this.heap = []
    }

    push(value: number, index: number): void {
        this.heap.push([value, index])
        this.heap.sort((a, b) => b[0] - a[0])
    }

    pop(): void {
        this.heap.shift()
    }

    peek(): number[] {
        return this.heap[0]
    }

    isEmpty(): boolean {
        return this.heap.length === 0
    }
}

export class Solution {
    maxSlidingWindow(nums: number[], k: number): number[] {
        const n = nums.length;
        const result: number[] = new Array(n - k + 1);
        const maxHeap: MaxHeap = new MaxHeap(); // Max-heap: [value, index]

        for (let i = 0; i < n; i++) {
            // Add the current element to the heap
            maxHeap.push(nums[i], i);

            // Remove elements that are out of the current window
            while (!maxHeap.isEmpty() && maxHeap.peek()[1] < i - k + 1) {
                maxHeap.pop();
            }

            // Add the maximum element of the current window to the result
            if (i >= k - 1) {
                result[i - k + 1] = maxHeap.peek()[0];
            }
        }

        return result;
    }
}
```

