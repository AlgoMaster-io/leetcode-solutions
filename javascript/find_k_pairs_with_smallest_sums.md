# 373. [Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/)

## Approach: Min-Heap

### Solution
```javascript
// Time Complexity: O(k * log(k)), where k is the number of pairs to find
// Space Complexity: O(k), for the heap

class Solution {
    kSmallestPairs(nums1, nums2, k) {
        // Min-heap to store pairs along with their sums
        const minHeap = new MinPriorityQueue({ 
            compare: (a, b) => (a[0] + a[1]) - (b[0] + b[1])
        });

        const result = [];

        // Initialize the heap with pairs (nums1[i], nums2[0]) for all i
        for (let i = 0; i < Math.min(nums1.length, k); i++) {
            minHeap.enqueue([nums1[i], nums2[0], 0]);
        }

        // Extract the smallest pairs until we have k pairs or the heap is empty
        while (!minHeap.isEmpty() && k > 0) {
            const current = minHeap.dequeue();
            result.push([current[0], current[1]]);
            k--;

            // If there are more pairs with the current nums1[i], add them to the heap
            if (current[2] + 1 < nums2.length) {
                minHeap.enqueue([current[0], nums2[current[2] + 1], current[2] + 1]);
            }
        }

        return result;
    }
}
```

