# [215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)

## Approach 1: Sorting

### Solution
typescript
```typescript
// Time Complexity: O(n log n)
// Space Complexity: O(1)
function findKthLargest(nums: number[], k: number): number {
    nums.sort((a, b) => a - b); // Sort the array in ascending order
    return nums[nums.length - k]; // Return the kth largest element
}
```

## Approach 2: Using a Min-Heap

### Solution
typescript
```typescript
// Time Complexity: O(n log k)
// Space Complexity: O(k)
class MinHeap {
    private heap: number[] = [];

    public size(): number {
        return this.heap.length;
    }

    private swap(i: number, j: number): void {
        const temp = this.heap[i];
        this.heap[i] = this.heap[j];
        this.heap[j] = temp;
    }

    private heapifyUp(): void {
        let index = this.heap.length - 1;
        while (index > 0) {
            const parentIndex = Math.floor((index - 1) / 2);
            if (this.heap[index] >= this.heap[parentIndex]) break;
            this.swap(index, parentIndex);
            index = parentIndex;
        }
    }

    private heapifyDown(): void {
        let index = 0;
        const length = this.heap.length;
        while ((2 * index + 1) < length) {
            let smallest = 2 * index + 1;
            if ((2 * index + 2) < length && this.heap[2 * index + 2] < this.heap[smallest]) {
                smallest = 2 * index + 2;
            }
            if (this.heap[index] <= this.heap[smallest]) break;
            this.swap(index, smallest);
            index = smallest;
        }
    }

    public offer(value: number): void {
        this.heap.push(value);
        this.heapifyUp();
    }

    public poll(): number | undefined {
        if (this.size() === 0) return undefined;
        this.swap(0, this.heap.length - 1);
        const min = this.heap.pop();
        this.heapifyDown();
        return min;
    }

    public peek(): number | undefined {
        return this.heap[0];
    }
}

function findKthLargest(nums: number[], k: number): number {
    const minHeap = new MinHeap();

    for (const num of nums) {
        minHeap.offer(num);
        if (minHeap.size() > k) {
            minHeap.poll();
        }
    }

    return minHeap.peek()!;
}
```

## Approach 3: Quickselect (Partitioning)

### Solution
typescript
```typescript
// Time Complexity: O(n) on average, O(n^2) in the worst case
// Space Complexity: O(1)
function findKthLargest(nums: number[], k: number): number {
    return quickSelect(nums, 0, nums.length - 1, nums.length - k);
}

function quickSelect(nums: number[], left: number, right: number, k: number): number {
    if (left === right) {
        return nums[left]; // Base case: only one element
    }

    const pivotIndex = left + Math.floor(Math.random() * (right - left + 1));
    const newPivotIndex = partition(nums, left, right, pivotIndex);

    if (newPivotIndex === k) {
        return nums[k]; // Found the kth largest element
    } else if (newPivotIndex < k) {
        return quickSelect(nums, newPivotIndex + 1, right, k); // Search in the right part
    } else {
        return quickSelect(nums, left, newPivotIndex - 1, k); // Search in the left part
    }
}

function partition(nums: number[], left: number, right: number, pivotIndex: number): number {
    const pivotValue = nums[pivotIndex];
    [nums[pivotIndex], nums[right]] = [nums[right], nums[pivotIndex]]; // Move pivot to end
    let storeIndex = left;

    for (let i = left; i < right; i++) {
        if (nums[i] < pivotValue) {
            [nums[storeIndex], nums[i]] = [nums[i], nums[storeIndex]];
            storeIndex++;
        }
    }

    [nums[storeIndex], nums[right]] = [nums[right], nums[storeIndex]]; // Move pivot to its final place
    return storeIndex;
}
```

