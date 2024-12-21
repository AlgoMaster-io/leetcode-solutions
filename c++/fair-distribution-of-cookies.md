# [Leetcode 2305: Fair Distribution of Cookies](https://leetcode.com/problems/fair-distribution-of-cookies/)

### Approaches:
1. [Backtracking](#backtracking)
2. [Min-Heap Optimization](#min-heap-optimization)

---

## Backtracking

The problem involves distributing cookies among kids such that the maximum number of cookies given to any kid is minimized. To solve this, initially consider a straightforward recursive backtracking approach:

- Distribute the cookies while considering all possible distributions.
- For each kid, distribute cookies and recursively find the best distribution for the remaining kids and cookies.
- Keep track of the maximum number of cookies a kid receives at any time across any valid distribution.
- Use backtracking to explore different permutation branches to find minimal maximum distribution.

### Intuition

1. **Recursive Exploration**: Think of each distribution attempt as a node in a tree; the tree expands as we distribute cookies to each kid and traverses deeper.
2. **Pruning**: If, at any point, the current maximum cookie count is already greater than a known result, stop the current exploration (pruning) as it’s not better.
3. **Recursive Base Condition**: Base condition is when all cookies are distributed.

### C++ Code:

```cpp
class Solution {
public:
    void distribute(int index, vector<int>& cookies, vector<int>& distribution, int& minUnfairness) {
        // If all cookies are distributed
        if (index == cookies.size()) {
            int maxCookies = *max_element(distribution.begin(), distribution.end());
            // Update the minimum unfairness (max value)
            minUnfairness = min(minUnfairness, maxCookies);
            return;
        }
        
        // Try giving the current cookie bag `index` to different kids
        for (int i = 0; i < distribution.size(); ++i) {
            distribution[i] += cookies[index]; // Give cookies to ith kid
            distribute(index + 1, cookies, distribution, minUnfairness); // Recurse
            distribution[i] -= cookies[index]; // Backtrack
        }
    }
    
    int distributeCookies(vector<int>& cookies, int k) {
        vector<int> distribution(k, 0); // Initialize the distribution
        int minUnfairness = INT_MAX; // Track the minimum of maximum cookies given
        distribute(0, cookies, distribution, minUnfairness);
        return minUnfairness;
    }
};
```

### Time and Space Complexity

- **Time Complexity**: O(k^n), where `n` is the number of cookie bags and `k` is the number of kids. We explore each cookie distribution combination.
- **Space Complexity**: O(k + n), for recursion stack and distribution array.

---

## Min-Heap Optimization

### Intuition

After exploring the exhaustive approach, we can optimize using a min-heap to keep track of which kid has received the least cookies. This approach can help in guiding the distribution process more optimally.

### Steps

1. Use a priority queue (min-heap) to get the current kid with the least cookies efficiently.
2. Distribute cookies to this kid from the min-heap to potentially reduce maximum unfairness.
3. Maintain an updated heap after every operation.

### C++ Code:

The optimized approach changes the nature of recursion to a greedy selection based on current least distribution and tries to optimize paths explored.

```cpp
#include <queue>

class Solution {
public:
    void distribute(int index, vector<int>& cookies, priority_queue<int, vector<int>, greater<int>>& minHeap, int k, int& minUnfairness) {
        if (index == cookies.size()) {
            int maxCookies = minHeap.top();
            minUnfairness = min(minUnfairness, maxCookies);
            return;
        }
        
        vector<int> snapshot;
        while (!minHeap.empty()) {
            snapshot.push_back(minHeap.top()); // Capture the current state
            minHeap.pop();
        }
        
        for (int i = 0; i < k; ++i) {
            snapshot[i] += cookies[index];
            for (int val : snapshot) minHeap.push(val); // Repopulate the heap
            distribute(index + 1, cookies, minHeap, k, minUnfairness); // Recurse
            minHeap = priority_queue<int, vector<int>, greater<int>>();
            snapshot[i] -= cookies[index];
            for (int val : snapshot) minHeap.push(val); // Restore state
        }
    }
    
    int distributeCookies(vector<int>& cookies, int k) {
        priority_queue<int, vector<int>, greater<int>> minHeap;
        for (int i = 0; i < k; ++i) minHeap.push(0);
        int minUnfairness = INT_MAX;
        distribute(0, cookies, minHeap, k, minUnfairness);
        return minUnfairness;
    }
};
```

### Time and Space Complexity

- **Time Complexity**: O(k^n), though potentially reduced in practice by guiding distribution better.
- **Space Complexity**: O(k), for the minHeap maintaining current distribution state.

