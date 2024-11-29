# 480. [Sliding Window Median](https://leetcode.com/problems/sliding-window-median/)

## Approach: Two Heaps (Max-Heap and Min-Heap)

### Solution
```cpp
// Time Complexity: O(n * log(k)), where n is the size of nums and k is the size of the window
// Space Complexity: O(k), for storing elements in the heaps
#include <vector>
#include <queue>

class Solution {
public:
    std::vector<double> medianSlidingWindow(std::vector<int>& nums, int k) {
        int n = nums.size();
        std::vector<double> result(n - k + 1);
        
        // Defining max-heap for the smaller half and min-heap for the larger half
        std::priority_queue<int> maxHeap; // max-heap
        std::priority_queue<int, std::vector<int>, std::greater<int>> minHeap; // min-heap
        
        for (int i = 0; i < n; i++) {
            // Add the new element into the appropriate heap
            if (maxHeap.empty() || nums[i] <= maxHeap.top()) {
                maxHeap.push(nums[i]);
            } else {
                minHeap.push(nums[i]);
            }
            
            // Rebalance the heaps if necessary
            if (maxHeap.size() > minHeap.size() + 1) {
                minHeap.push(maxHeap.top());
                maxHeap.pop();
            } else if (minHeap.size() > maxHeap.size()) {
                maxHeap.push(minHeap.top());
                minHeap.pop();
            }
            
            // Remove the element that is sliding out of the window
            if (i >= k) {
                int elementToRemove = nums[i - k];
                if (elementToRemove <= maxHeap.top()) {
                    maxHeap.erase(maxHeap.find(elementToRemove));
                } else {
                    minHeap.erase(minHeap.find(elementToRemove));
                }
                
                // Rebalance the heaps after removal
                if (maxHeap.size() > minHeap.size() + 1) {
                    minHeap.push(maxHeap.top());
                    maxHeap.pop();
                } else if (minHeap.size() > maxHeap.size()) {
                    maxHeap.push(minHeap.top());
                    minHeap.pop();
                }
            }
            
            // Add the median to the result when the window is fully formed
            if (i >= k - 1) {
                if (maxHeap.size() == minHeap.size()) {
                    result[i - k + 1] = ((double)maxHeap.top() + minHeap.top()) / 2;
                } else {
                    result[i - k + 1] = maxHeap.top();
                }
            }
        }
        
        return result;
    }
};
```

