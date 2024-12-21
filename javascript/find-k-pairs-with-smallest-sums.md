[Find K Pairs with Smallest Sums - LeetCode](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/)

## Approaches
1. [Brute Force with Sorting](#approach-1-brute-force-with-sorting)
2. [Optimized Approach using Min-Heap](#approach-2-optimized-approach-using-min-heap)

### Approach 1: Brute Force with Sorting
This approach involves a simple brute-force method where we generate all possible pairs from the two input arrays, calculate their sums, and then sort these sums to find the k smallest ones.

#### Intuition
The idea is straightforward: generate all possible pairs from the two arrays and store their sums in another array along with the pair themselves. Then, sort this array based on the sums and simply pick the first k elements.

#### Implementation
```javascript
function kSmallestPairs(nums1, nums2, k) {
    // Create a list to store all possible pairs with their sums
    let pairs = [];
    
    // Iterate through all possible pairs
    for (let i = 0; i < nums1.length; i++) {
        for (let j = 0; j < nums2.length; j++) {
            // Store each pair and their sums
            pairs.push([nums1[i], nums2[j], nums1[i] + nums2[j]]);
        }
    }
    
    // Sorting pairs based on the sum
    pairs.sort((a, b) => a[2] - b[2]);
    
    // Extract the first k pairs
    let result = [];
    for (let m = 0; m < Math.min(k, pairs.length); m++) {
        result.push([pairs[m][0], pairs[m][1]]);
    }
    
    return result;
}
```

#### Time Complexity
- **Time:** O(m * n * log(m * n)), where m is the length of `nums1` and n is the length of `nums2`. The time complexity is mainly due to sorting the m*n pairs.
  
#### Space Complexity
- **Space:** O(m * n), used to store all pairs.

---

### Approach 2: Optimized Approach using Min-Heap
This approach optimizes the brute-force method by using a min-heap (priority queue) to efficiently get the k smallest sums without explicitly sorting all pairs.

#### Intuition
Instead of computing all pairs at once, we can utilize a min-heap to efficiently manage the smallest pairs while dynamically exploring potential pairs. Start with the smallest possible sums, push neighbors into the heap to explore, and always extract the smallest sum pair from the heap until k pairs are found.

#### Implementation
```javascript
function kSmallestPairs(nums1, nums2, k) {
    // Ensure priority queue via a min-heap
    const minHeap = [];
    const result = [];
    
    // Initialize the min-heap with the first element combination from nums1 with the first element in nums2
    for (let i = 0; i < Math.min(nums1.length, k); i++) {
        minHeap.push({ sum: nums1[i] + nums2[0], i: i, j: 0 });
    }
    
    // Function to maintain the heap's min-heap property
    const minHeapify = (heap) => {
        heap.sort((a, b) => a.sum - b.sum);
    };
    
    // Extract k smallest pairs
    while (result.length < k && minHeap.length > 0) {
        // Sort to maintain min-heap order
        minHeapify(minHeap);
        
        // Extract the smallest sum pair
        const { i, j } = minHeap.shift();
        result.push([nums1[i], nums2[j]]);
        
        // Push next pair with indices i and j+1 if within bounds
        if (j + 1 < nums2.length) {
            minHeap.push({ sum: nums1[i] + nums2[j + 1], i: i, j: j + 1 });
        }
    }
    
    return result;
}
```

#### Time Complexity
- **Time:** O(k * log(min(m, k))), since we limit heap size to the lesser of m or k.

#### Space Complexity
- **Space:** O(min(m, n, k)), due to heap storage and result storage of size k. 

Both approaches present a clear progression from simple brute-force to an efficient solution using data structures like the min-heap to manage pair comparisons optimally.

