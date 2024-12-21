# [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)

## Approaches
1. [Naive Sorting Approach](#naive-sorting-approach)
2. [Min-Heap Approach](#min-heap-approach)
3. [Quickselect Approach](#quickselect-approach)

---

### Naive Sorting Approach

#### Intuition
The straightforward way to solve this problem is by sorting the array in descending order and then simply picking the k-th element from this sorted array. This approach is easy to implement but not optimal in terms of time complexity for large arrays.

#### Code
```typescript
function findKthLargest(nums: number[], k: number): number {
    // Sort the array in descending order
    nums.sort((a, b) => b - a);
    // Return the k-th element (index k-1)
    return nums[k - 1];
}
```

#### Complexity
- **Time Complexity**: O(n log n), due to sorting the array.
- **Space Complexity**: O(1), as we are sorting the array in place.

---

### Min-Heap Approach

#### Intuition
A more efficient way is to use a min-heap (or priority queue) with a size of k. By maintaining a min-heap of size k, we ensure that the root of the heap is the k-th largest element after processing all elements. We achieve this by iterating through the array and adding elements to the heap, and if the heap size exceeds k, we remove the smallest element.

#### Code
```typescript
function findKthLargest(nums: number[], k: number): number {
    const heap = new MinHeap<number>();

    for (let num of nums) {
        heap.insert(num);
        if (heap.size() > k) {
            heap.extractMin();
        }
    }

    return heap.peek();
}

class MinHeap<T> {
    private heap: T[] = [];
    
    insert(value: T) {
        this.heap.push(value);
        this.bubbleUp(this.heap.length - 1);
    }

    extractMin(): T | undefined {
        const min = this.heap[0];
        const end = this.heap.pop();
        if (this.heap.length > 0 && end !== undefined) {
            this.heap[0] = end;
            this.bubbleDown(0);
        }
        return min;
    }

    peek(): T | undefined {
        return this.heap[0];
    }

    size(): number {
        return this.heap.length;
    }

    private bubbleUp(index: number) {
        while (index > 0) {
            let parentIndex = Math.floor((index - 1) / 2);
            if (this.heap[index] >= this.heap[parentIndex]) break;
            [this.heap[index], this.heap[parentIndex]] = [this.heap[parentIndex], this.heap[index]];
            index = parentIndex;
        }
    }
    
    private bubbleDown(index: number) {
        const length = this.heap.length;
        const element = this.heap[index];

        while (true) {
            let leftIdx = 2 * index + 1;
            let rightIdx = 2 * index + 2;
            let left: T, right: T;

            let swap = null;

            if (leftIdx < length) {
                left = this.heap[leftIdx];
                if (left < element) {
                    swap = leftIdx;
                }
            }

            if (rightIdx < length) {
                right = this.heap[rightIdx];
                if (
                    (swap === null && right < element) || 
                    (swap !== null && right < left!)
                ) {
                    swap = rightIdx;
                }
            }

            if (swap === null) break;
            [this.heap[index], this.heap[swap]] = [this.heap[swap], this.heap[index]];
            index = swap;
        }
    }
}
```

#### Complexity
- **Time Complexity**: O(n log k), where n is the number of elements, since we maintain a heap of size k.
- **Space Complexity**: O(k), due to the space required for the heap.

---

### Quickselect Approach

#### Intuition
Quickselect is a selection algorithm to find the k-th smallest element in an unordered list. It is related to the quicksort sorting algorithm. The key idea is to use the partitioning step from quicksort, which partitions an array into elements less than a pivot and elements greater than a pivot. By choosing a pivot and partitioning the array, we can determine which side of the pivot the k-th largest element lies on, and recursively continue our search on that side.

#### Code
```typescript
function findKthLargest(nums: number[], k: number): number {
    const targetIndex = nums.length - k;

    const quickSelect = (left: number, right: number): number => {
        const pivotIndex = partition(nums, left, right);

        if (pivotIndex === targetIndex) {
            return nums[pivotIndex];
        } else if (pivotIndex < targetIndex) {
            return quickSelect(pivotIndex + 1, right);
        } else {
            return quickSelect(left, pivotIndex - 1);
        }
    };

    const partition = (nums: number[], left: number, right: number): number => {
        const pivot = nums[right];
        let i = left;

        for (let j = left; j < right; j++) {
            if (nums[j] <= pivot) {
                [nums[i], nums[j]] = [nums[j], nums[i]];
                i++;
            }
        }
        [nums[i], nums[right]] = [nums[right], nums[i]];
        return i;
    };

    return quickSelect(0, nums.length - 1);
}
```

#### Complexity
- **Time Complexity**: Average case O(n), but worst-case O(n^2). The average performance is achieved due to a good pivot selection strategy.
- **Space Complexity**: O(1), if we disregard the recursion stack space.

This problem illustrates different approaches to tackling the same problem, each with its trade-offs in terms of speed and space efficiency. Quickselect is often preferred due to its average case optimal performance, but requires careful implementation and understanding of its recursive partitioning strategy.

