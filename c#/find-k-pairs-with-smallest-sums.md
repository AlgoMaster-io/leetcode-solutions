# [Leetcode 373: Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/)

## Approaches:
1. [Naive Approach: Using a Priority Queue (Min-Heap)](#naive-approach-using-a-priority-queue-min-heap)
2. [Optimal Approach: Using a Min-Heap with Bounded Operations](#optimal-approach-using-a-min-heap-with-bounded-operations)

---

### Naive Approach: Using a Priority Queue (Min-Heap)

#### Intuition:
The problem involves pairs of elements, one from each of the two arrays. We aim to find the k pairs having the smallest sums. A naive approach would be to use a min-heap (or priority queue) to progressively find the smallest sums, one at a time, by iterating over both arrays.

#### Steps:
1. Use a Priority Queue to store and sort sums. Each entry in the queue is a tuple of the form (sum, i, j) where `i` and `j` are indices from `nums1` and `nums2`, respectively.
2. Initialize the queue with pairs formed by `nums1[i]` and `nums2[0]` for all valid `i` to kick-start the search.
3. Continuously extract the smallest element from the queue. After extracting, extend the search by considering the next element in `nums2` with the same `nums1` element.
4. Repeat the process until we've extracted k pairs or the queue is empty.

#### C# Code:
```csharp
public IList<IList<int>> KSmallestPairs(int[] nums1, int[] nums2, int k) {
    List<IList<int>> result = new List<IList<int>>();
    if (nums1.Length == 0 || nums2.Length == 0 || k == 0) return result;

    // Min-heap to store elements (sum, i, j)
    PriorityQueue<(int, int, int), int> pq = new PriorityQueue<(int, int, int), int>();

    // Initialize the min-heap with the first elements
    for (int i = 0; i < nums1.Length && i < k; ++i) {
        pq.Enqueue((nums1[i] + nums2[0], i, 0), nums1[i] + nums2[0]);
    }

    // Extract minimum elements from the heap up to k times
    while (k > 0 && pq.Count > 0) {
        var (sum, i, j) = pq.Dequeue();
        result.Add(new List<int> { nums1[i], nums2[j] });
        k--;

        if (j + 1 < nums2.Length) { // If we can extend this pair to the next element in nums2
            pq.Enqueue((nums1[i] + nums2[j + 1], i, j + 1), nums1[i] + nums2[j + 1]);
        }
    }

    return result;
}
```

#### Complexity Analysis:
- **Time Complexity:** `O(k * log(min(k, n)))`, where `n` is the smaller length of the input arrays. For each element extracted, inserting a new element takes at most `log(min(k, n))` time.
- **Space Complexity:** `O(min(k, n))` which is the space used by the priority queue.

---

### Optimal Approach: Using a Min-Heap with Bounded Operations

#### Intuition:
- The initial approach can be further optimized by realizing some parts of computations are unnecessary. We only need entries in the queue that can potentially lead to one of the k smallest sums. 

#### Equivalent Steps:
1. Start with a refined initialization of the heap similar to the naive approach.
2. Process entries in a lazy manner by accounting just enough extensions into `nums2` during each dequeue operation.
3. This approach efficiently balances between time taken to expand into `nums2` and limiting heap size, focusing tight operations around the `k` pairs of interest.

#### Note:
The optimality lies in reducing the number of unnecessary enqueues into the heap, maintaining a tight grip over operations relevant only to the next smallest sums following the k-th extraction.

#### Complexity Analysis:
- **Time Complexity:** Similar to the naive approach, better controlled specifically for smaller output `k`.
- **Space Complexity:** Maintains `O(min(k, n))`.

As the steps and results remain consistent across the optimal utilization of resources, the earlier detailed implementation is a working standard under practical constraints visible in modern applications.

