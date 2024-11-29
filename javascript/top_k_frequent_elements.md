# 347. [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

## Approach 1: Min-Heap

### Solution
javascript
```javascript
// Time Complexity: O(n * log(k)), where n is the number of elements in nums
// Space Complexity: O(n), for the frequency map and heap

function topKFrequent(nums, k) {
    // Step 1: Build a frequency map
    const frequencyMap = new Map();
    nums.forEach(num => {
        frequencyMap.set(num, (frequencyMap.get(num) || 0) + 1);
    });

    // Step 2: Use a min-heap to keep track of the top k elements
    const minHeap = [];

    frequencyMap.forEach((value, key) => {
        minHeap.push([key, value]);
        minHeap.sort((a, b) => a[1] - b[1]); // Keep min-heap sorted by frequency

        if (minHeap.length > k) {
            minHeap.shift(); // Remove the element with the smallest frequency
        }
    });

    // Step 3: Extract elements from the heap
    return minHeap.map(item => item[0]);
}
```

## Approach 2: Bucket Sort

### Solution
javascript
```javascript
// Time Complexity: O(n), where n is the number of elements in nums
// Space Complexity: O(n), for the frequency map and buckets

function topKFrequent(nums, k) {
    // Step 1: Build a frequency map
    const frequencyMap = new Map();
    nums.forEach(num => {
        frequencyMap.set(num, (frequencyMap.get(num) || 0) + 1);
    });

    // Step 2: Create buckets where index represents frequency
    const buckets = Array.from({ length: nums.length + 1 }, () => []);

    frequencyMap.forEach((frequency, key) => {
        buckets[frequency].push(key);
    });

    // Step 3: Collect the top k frequent elements
    const result = [];
    for (let i = buckets.length - 1; i >= 0 && result.length < k; i--) {
        if (buckets[i].length > 0) {
            result.push(...buckets[i]);
            if (result.length >= k) {
                return result.slice(0, k);
            }
        }
    }

    return result;
}
```

