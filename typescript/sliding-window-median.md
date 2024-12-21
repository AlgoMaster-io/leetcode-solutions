[Leetcode 480: Sliding Window Median](https://leetcode.com/problems/sliding-window-median/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Approach using Heaps](#optimized-approach-using-heaps)

### Brute Force Approach

#### Intuition

The brute force solution involves iterating over each window, sorting the elements within the window, and then calculating the median. While straightforward, this approach is not optimal due to the repeated sorting operation for each window shift.

#### Steps:
1. Iterate through the list with a window of size `k`.
2. For each window, extract the current subset of elements.
3. Sort these elements and then calculate the median. If the window size `k` is odd, take the middle element. If it's even, take the average of the two middle elements.
4. Store the median in the results array.

#### Code

```typescript
function medianSlidingWindow(nums: number[], k: number): number[] {
    const result: number[] = [];
    
    for (let i = 0; i <= nums.length - k; i++) {
        // Extract the window
        const window = nums.slice(i, i + k);
        
        // Sort the window elements
        window.sort((a, b) => a - b);
        
        // Calculate median based on even/odd length
        if (k % 2 === 0) {
            result.push((window[k / 2 - 1] + window[k / 2]) / 2);
        } else {
            result.push(window[Math.floor(k / 2)]);
        }
    }
    
    return result;
}
```

#### Complexity:
- **Time Complexity**: O(n * k log k), where `n` is the length of the array, as sorting the window requires O(k log k).
- **Space Complexity**: O(k) due to the space needed for the window slice.

---

### Optimized Approach using Heaps

#### Intuition

This approach uses two heaps (a max-heap and a min-heap) to efficiently track the elements in the window, which allows us to get the median in constant time. The max-heap stores the lower half of the numbers, whereas the min-heap stores the upper half. The balancing of the heaps ensures that the median can be extracted easily.

#### Steps:
1. Use two heaps: a max-heap for the lower half of the window and a min-heap for the upper half.
2. Add new elements to the heaps while maintaining the heaps' properties.
3. Remove elements that are sliding out of the window.
4. Balance the heaps if necessary to ensure the difference in size is at most one. Make sure all elements in the max-heap are less than or equal to the elements in the min-heap.
5. Calculate the median based on the heaps' sizes and add it to the result.

#### Code

```typescript
class Heap {
    private data: number[];
    private comparator: (a: number, b: number) => boolean;

    constructor(comparator: (a: number, b: number) => boolean) {
        this.data = [];
        this.comparator = comparator;
    }

    push(val: number) {
        this.data.push(val);
        this.bubbleUp();
    }
    
    pop(): number | undefined {
        const top = this.peek();
        const bottom = this.data.pop();
        if (this.data.length > 0 && bottom !== undefined) {
            this.data[0] = bottom;
            this.bubbleDown();
        }
        return top;
    }

    peek(): number | undefined {
        return this.data.length > 0 ? this.data[0] : undefined;
    }

    size(): number {
        return this.data.length;
    }

    private bubbleUp() {
        let index = this.data.length - 1;
        const value = this.data[index];

        while (index > 0) {
            const parentIndex = Math.floor((index - 1) / 2);
            const parentValue = this.data[parentIndex];

            if (!this.comparator(value, parentValue)) break;

            this.data[index] = parentValue;
            index = parentIndex;
        }
        this.data[index] = value;
    }

    private bubbleDown() {
        let index = 0;
        const length = this.data.length;
        const value = this.data[index];

        while (true) {
            let leftChildIndex = 2 * index + 1;
            let rightChildIndex = 2 * index + 2;
            let swapIndex = index;

            if (leftChildIndex < length && this.comparator(this.data[leftChildIndex], this.data[swapIndex])) {
                swapIndex = leftChildIndex;
            }
            if (rightChildIndex < length && this.comparator(this.data[rightChildIndex], this.data[swapIndex])) {
                swapIndex = rightChildIndex;
            }

            if (swapIndex === index) break;

            this.data[index] = this.data[swapIndex];
            index = swapIndex;
        }
        this.data[index] = value;
    }
}

function medianSlidingWindow(nums: number[], k: number): number[] {
    const minHeap = new Heap((a, b) => a < b); // Min-heap for right half
    const maxHeap = new Heap((a, b) => a > b); // Max-heap for left half
    const result: number[] = [];

    const addNum = (num: number) => {
        maxHeap.push(num);
        minHeap.push(maxHeap.pop()!); // Ensure the smallest from maxHeap goes to minHeap

        if (minHeap.size() > maxHeap.size()) {
            maxHeap.push(minHeap.pop()!); // Balance the sizes
        }
    };

    const removeNum = (num: number) => {
        // Remove a number by lazy deletion
        if (num <= maxHeap.peek()!) {
            maxHeap.pop();
        } else {
            minHeap.pop();
        }
    };

    const findMedian = (): number => {
        // Get the median
        if (maxHeap.size() > minHeap.size()) {
            return maxHeap.peek()!;
        } else {
            return (maxHeap.peek()! + minHeap.peek()!) / 2;
        }
    };

    for (let i = 0; i < nums.length; i++) {
        addNum(nums[i]);
        
        if (i >= k - 1) {
            result.push(findMedian());
            removeNum(nums[i - k + 1]);
        }
    }

    return result;
}
```

#### Complexity:
- **Time Complexity**: O(n log k), where `n` is the length of the array and `k` is the window size, as each add/remove operation on the heaps takes O(log k).
- **Space Complexity**: O(k), needed for storing elements in the heaps.

This optimized approach performs significantly better than the brute-force approach, especially for larger values of `k` and `n`.

