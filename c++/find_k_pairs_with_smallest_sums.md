# 373. [Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/)

## Approach: Min-Heap

### Solution
c++
// Time Complexity: O(k * log(k)), where k is the number of pairs to find
// Space Complexity: O(k), for the heap
```cpp
#include <vector>
#include <queue>

using namespace std;

class Solution {
public:
    vector<vector<int>> kSmallestPairs(vector<int>& nums1, vector<int>& nums2, int k) {
        // Min-heap to store pairs along with their sums
        auto cmp = [](const vector<int>& a, const vector<int>& b) {
            return a[0] + a[1] > b[0] + b[1];
        };
        priority_queue<vector<int>, vector<vector<int>>, decltype(cmp)> minHeap(cmp);

        vector<vector<int>> result;

        // Initialize the heap with pairs (nums1[i], nums2[0]) for all i
        for (int i = 0; i < min(nums1.size(), (size_t)k); i++) {
            minHeap.push({nums1[i], nums2[0], 0});
        }

        // Extract the smallest pairs until we have k pairs or the heap is empty
        while (!minHeap.empty() && k > 0) {
            vector<int> current = minHeap.top();
            minHeap.pop();
            result.push_back({current[0], current[1]});
            k--;

            // If there are more pairs with the current nums1[i], add them to the heap
            if (current[2] + 1 < nums2.size()) {
                minHeap.push({current[0], nums2[current[2] + 1], current[2] + 1});
            }
        }

        return result;
    }
};
```

