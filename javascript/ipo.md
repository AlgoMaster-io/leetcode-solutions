# 480. [Sliding Window Median](https://leetcode.com/problems/sliding-window-median/)

## Approach: Two Heaps (Max-Heap and Min-Heap)

### Solution
```javascript
// Time Complexity: O(n * log(k)), where n is the size of nums and k is the size of the window
// Space Complexity: O(k), for storing elements in the heaps
class Solution {
    medianSlidingWindow(nums, k) {
        const n = nums.length;
        const result = new Array(n - k + 1);
        
        // Max-heap for the smaller half
        const maxHeap = new MinPriorityQueue({
            priority: x => -x
        });
        // Min-heap for the larger half
        const minHeap = new MinPriorityQueue();

        for (let i = 0; i < n; i++) {
            // Add the new element into the appropriate heap
            if (maxHeap.size() === 0 || nums[i] <= maxHeap.front().element) {
                maxHeap.enqueue(nums[i]);
            } else {
                minHeap.enqueue(nums[i]);
            }
            
            // Rebalance the heaps if necessary
            if (maxHeap.size() > minHeap.size() + 1) {
                minHeap.enqueue(maxHeap.dequeue().element);
            } else if (minHeap.size() > maxHeap.size()) {
                maxHeap.enqueue(minHeap.dequeue().element);
            }
            
            // Remove the element that is sliding out of the window
            if (i >= k) {
                const elementToRemove = nums[i - k];
                if (elementToRemove <= maxHeap.front().element) {
                    // Find and remove the element in the maxHeap
                    this.removeElement(maxHeap, elementToRemove);
                } else {
                    // Find and remove the element in the minHeap
                    this.removeElement(minHeap, elementToRemove);
                }
                
                // Rebalance the heaps after removal
                if (maxHeap.size() > minHeap.size() + 1) {
                    minHeap.enqueue(maxHeap.dequeue().element);
                } else if (minHeap.size() > maxHeap.size()) {
                    maxHeap.enqueue(minHeap.dequeue().element);
                }
            }
            
            // Add the median to the result when the window is fully formed
            if (i >= k - 1) {
                if (maxHeap.size() === minHeap.size()) {
                    result[i - k + 1] = (maxHeap.front().element + minHeap.front().element) / 2;
                } else {
                    result[i - k + 1] = maxHeap.front().element;
                }
            }
        }
        
        return result;
    }

    removeElement(heap, value) {
        let foundIndex = -1;
        
        // Find the value in the heap
        for (let i = 0; i < heap.size(); i++) {
            if (heap._heap[i].element === value) {
                foundIndex = i;
                break;
            }
        }
        
        if (foundIndex !== -1) {
            // Remove the element and rebuild the heap
            heap._heap[foundIndex] = heap._heap[heap.size() - 1];
            heap._heap.pop();
            heap._forwardHeapify();
        }
    }
}
```

Note: This JavaScript implementation uses a priority queue library that provides `MinPriorityQueue` and `MaxPriorityQueue`. Libraries such as `datastructures-js` or custom implementations can fulfill this purpose since JavaScript does not include priority queue structures natively.

