# [373. Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/)

## Table of Contents
1. [Approach 1: Brute Force](#approach-1)
2. [Approach 2: Min-Heap Optimization](#approach-2)
3. [Approach 3: Optimized Min-Heap with Index Tracking](#approach-3)

### Approach 1: Brute Force

**Intuition**:

- Start by generating all possible pairs (i, j) from `nums1` and `nums2`.
- Calculate the sum for each pair and store these results along with the pairs.
- Sort the array of results based on their sums.
- Pick the first `k` pairs from this sorted list.

**Time Complexity**: O(n * m * log(n * m))  
**Space Complexity**: O(n * m)

```cpp
#include <vector>
#include <algorithm>

std::vector<std::vector<int>> kSmallestPairs(std::vector<int>& nums1, std::vector<int>& nums2, int k) {
    std::vector<std::vector<int>> pairs;
    
    // Generate all pairs and their sums
    for (int i = 0; i < nums1.size(); ++i) {
        for (int j = 0; j < nums2.size(); ++j) {
            pairs.push_back({nums1[i], nums2[j]});
        }
    }
    
    // Sort pairs based on their sums
    std::sort(pairs.begin(), pairs.end(), [](std::vector<int>& a, std::vector<int>& b) {
        return a[0] + a[1] < b[0] + b[1];
    });
    
    // Take the first k pairs
    pairs.resize(std::min(k, (int)pairs.size()));
    return pairs;
}
```

### Approach 2: Min-Heap Optimization

**Intuition**:

- Use a min-heap to keep track of the smallest pairs.
- Start by pushing pairs from the first element of `nums1` with all elements of `nums2` into the heap.
- Extract from the heap until we get our k pairs.

**Time Complexity**: O(k * log(min(k, n * m)))  
**Space Complexity**: O(min(n, m) + k)

```cpp
#include <vector>
#include <queue>

std::vector<std::vector<int>> kSmallestPairs(std::vector<int>& nums1, std::vector<int>& nums2, int k) {
    using pii = std::pair<int, std::pair<int, int>>;
    std::priority_queue<pii, std::vector<pii>, std::greater<pii>> minHeap;

    // Push initial pairs (first element of nums1 with all elements of nums2)
    for (int i = 0; i < nums1.size() && i < k; ++i) {
        minHeap.push({nums1[i] + nums2[0], {i, 0}});
    }

    std::vector<std::vector<int>> result;
    while (k-- > 0 && !minHeap.empty()) {
        auto [sum, indices] = minHeap.top();
        minHeap.pop();
        int i = indices.first, j = indices.second;

        result.push_back({nums1[i], nums2[j]});

        // If there's a next element in nums2 for the current element in nums1, push it
        if (j + 1 < nums2.size()) {
            minHeap.push({nums1[i] + nums2[j + 1], {i, j + 1}});
        }
    }
    
    return result;
}
```

### Approach 3: Optimized Min-Heap with Index Tracking

**Intuition**:

- Instead of starting with all possible first pairs, track each index progress in `nums2` while extracting minimum pairs.
- Prevent repetitive computations by ensuring that each index progresses linearly in `nums2`.

**Time Complexity**: O(k * log(min(k, n * m)))  
**Space Complexity**: O(min(n, m) + k)

```cpp
#include <vector>
#include <queue>

std::vector<std::vector<int>> kSmallestPairs(std::vector<int>& nums1, std::vector<int>& nums2, int k) {
    using pii = std::pair<int, std::pair<int, int>>;
    std::priority_queue<pii, std::vector<pii>, std::greater<pii>> minHeap;

    std::vector<std::vector<int>> result;

    // Push pairs from nums1[i] and nums2[0] initially
    for (int i = 0; i < std::min((int)nums1.size(), k); ++i) {
        minHeap.push({nums1[i] + nums2[0], {i, 0}});
    }

    while (k-- > 0 && !minHeap.empty()) {
        auto [sum, indices] = minHeap.top();
        minHeap.pop();
        
        int i = indices.first, j = indices.second;
        result.push_back({nums1[i], nums2[j]});

        // If there is a next element in nums2 for the current index i in nums1, add it to the heap
        if (j + 1 < nums2.size()) {
            minHeap.push({nums1[i] + nums2[j + 1], {i, j + 1}});
        }
    }

    return result;
}
```

Each approach increases in complexity and efficiency, ultimately reaching a solution that efficiently tracks and computes using a priority-based system, tailored for finding the k smallest pairs efficiently.

