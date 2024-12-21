# [Leetcode 373: Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Optimized Approach using Min-Heap](#optimized-approach-using-min-heap)

---

### Brute Force Approach

**Intuition:**

The brute force approach is straightforward: generate all possible pairs from arrays `nums1` and `nums2`, sort these pairs based on their sum, and then select the first `k` pairs. This approach involves iterating over both arrays completely.

**Steps:**

1. Generate all pairs `(nums1[i], nums2[j])` for every element in both arrays.
2. Calculate the sum for each pair and store them in a list.
3. Sort the list of pairs based on the computed sum values.
4. Select the first `k` entries from the sorted list.

**Time Complexity:** O(m * n * log(m * n)) where `m` and `n` are the lengths of `nums1` and `nums2`, respectively.

**Space Complexity:** O(m * n) for storing all pairs.

```python
def kSmallestPairs(nums1, nums2, k):
    # Store pairs along with their sums
    pairs = []
    
    # Generate all combinations
    for i in range(len(nums1)):
        for j in range(len(nums2)):
            pairs.append((nums1[i], nums2[j]))
    
    # Sort pairs according to their sums
    pairs.sort(key=lambda x: x[0] + x[1])
    
    # Return the first k pairs
    return pairs[:k]

# Example usage
# nums1 = [1,2], nums2 = [3], k = 3
# This will return [(1, 3), (2, 3)]
```

---

### Optimized Approach using Min-Heap

**Intuition:**

To optimize, we use a Min-Heap to store the smallest possible sums and efficiently find and remove the smallest pair sum. By starting with the smallest element in `nums1` and iterating through each element in `nums2`, we only keep track of potential smallest sums.

**Steps:**

1. Initialize a Min-Heap.
2. Store initial pairs as `(nums1[i], nums2[0])` in the heap since pairing with the smallest element from `nums2` is likely to yield small sums.
3. Continuously extract the smallest sum pair from the heap and add the next potential sum pair with the next element in `nums2`.
4. Continue this process until you've extracted `k` sums or exhausted the list.
  
**Time Complexity:** O(k * log(min(m, n))) due to k extractions from the heap.

**Space Complexity:** O(k) to store the k smallest sums.

```python
import heapq

def kSmallestPairs(nums1, nums2, k):
    if not nums1 or not nums2 or k == 0:
        return []
    
    # Min heap to maintain the order of pairs with the smallest sums
    minHeap = []
    
    # Initialize the heap with pairs from nums1 with the first element in nums2
    for i in range(min(len(nums1), k)):  # Only need first `k` elements as the heap size constraint
        heapq.heappush(minHeap, (nums1[i] + nums2[0], i, 0))  # (sum, index in nums1, index in nums2)
    
    result = []
    
    while k > 0 and minHeap:
        # Get the pair with the smallest sum
        current_sum, i, j = heapq.heappop(minHeap)
        result.append((nums1[i], nums2[j]))
        
        # Move to the next pair by increasing the index in nums2
        if j + 1 < len(nums2):
            heapq.heappush(minHeap, (nums1[i] + nums2[j + 1], i, j + 1))
        
        # Decrement k after tracing one smallest pair
        k -= 1

    return result

# Example usage
# nums1 = [1,7,11], nums2 = [2,4,6], k = 3
# This will return [(1, 2), (1, 4), (1, 6)]
```

In the optimized approach using a min-heap, we avoid generating all possible pairs, significantly reducing both time and space complexity when m and n are large. This approach efficiently finds the K smallest pairs based on sum.

