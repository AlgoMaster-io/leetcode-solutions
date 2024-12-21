[Leetcode Problem 480: Sliding Window Median](https://leetcode.com/problems/sliding-window-median/)

Solution Approaches:
- [Solution 1: Brute Force with Sorting](#solution-1-brute-force-with-sorting)
- [Solution 2: Using Two Heaps (Min-Heap and Max-Heap)](#solution-2-using-two-heaps-min-heap-and-max-heap)

---

### Solution 1: Brute Force with Sorting

#### Intuition
The naive approach is to use a sliding window of size `k` and, for each window position, calculate the median by sorting the elements within that window. This requires extracting each window and sorting it to determine the median.

#### Detailed Explanation
1. **Iterate through the Array**: Use a loop to iterate over the `nums` array to cover all possible sliding window positions of size `k`.
2. **Extract and Sort**: For each window, extract the subarray and sort it.
3. **Find Median**: Determine the median of the sorted window. If `k` is odd, the median is the middle element. If `k` is even, the median is the average of the two middle elements.

#### Code
```cpp
#include <vector>
#include <algorithm>

class Solution {
public:
    std::vector<double> medianSlidingWindow(std::vector<int>& nums, int k) {
        std::vector<double> medians;
        std::vector<int> window;
        
        for (size_t i = 0; i <= nums.size() - k; ++i) {
            // Extract the current window
            window = std::vector<int>(nums.begin() + i, nums.begin() + i + k);
            
            // Sort the current window
            std::sort(window.begin(), window.end());
            
            // Find and append the median
            if (k % 2 == 1) {
                medians.push_back(window[k / 2]);
            } else {
                medians.push_back((window[k / 2 - 1] + window[k / 2]) / 2.0);
            }
        }
        
        return medians;
    }
};
```

#### Time Complexity
- Sorting each window of size `k` takes `O(k log k)`.
- Sliding over `n-k+1` possible windows takes `O((n-k+1) * k log k)`.
  
**Overall**: `O(nk log k)`

#### Space Complexity
- Space for storing each window is `O(k)`.

---

### Solution 2: Using Two Heaps (Min-Heap and Max-Heap)

#### Intuition
A more optimal solution involves using two heaps to maintain the window's elements: a max-heap to store the lower half of numbers and a min-heap to store the upper half. This allows efficient extraction of the median without needing to sort each time the window slides.

#### Detailed Explanation
1. **Two Heaps**: Use a max-heap to keep the lower half of the numbers, and a min-heap for the upper half.
2. **Balance Heaps**: Ensure the heaps are balanced so the number of elements in the max-heap is equal or one more than the min-heap.
3. **Sliding Window**: For each new element sliding into the window, add it to one of the heaps and remove the outgoing element.
4. **Get Median**:
   - If the heaps are balanced in size, the median is the average of the top elements of the two heaps.
   - If the max-heap is larger, the median is the top of the max-heap.

#### Code
```cpp
#include <vector>
#include <queue>
#include <unordered_map>

class Solution {
public:
    std::vector<double> medianSlidingWindow(std::vector<int>& nums, int k) {
        std::vector<double> medians;
        std::priority_queue<int> maxHeap; // Max-heap for lower half
        std::priority_queue<int, std::vector<int>, std::greater<int>> minHeap; // Min-heap for upper half
        std::unordered_map<int, int> invalidCount; // Track elements to discard

        // Lambda function to add a number to the heaps
        auto addNum = [&](int num) {
            if (maxHeap.empty() || num <= maxHeap.top()) {
                maxHeap.push(num);
            } else {
                minHeap.push(num);
            }
            balanceHeaps();
        };

        // Lambda function to remove a number and update heap sizes accordingly
        auto removeNum = [&](int num) {
            invalidCount[num]++;
            if (num <= maxHeap.top()) {
                maxHeapSize--;
            } else {
                minHeapSize--;
            }
            balanceHeaps();
        };

        // Recalculate balances and remove any invalid elements
        auto balanceHeaps = [&]() {
            while (maxHeapSize > minHeapSize + 1) {
                minHeap.push(maxHeap.top());
                maxHeap.pop();
                maxHeapSize--;
                minHeapSize++;
            }
            while (maxHeapSize < minHeapSize) {
                maxHeap.push(minHeap.top());
                minHeap.pop();
                minHeapSize--;
                maxHeapSize++;
            }
            // Remove any outdated elements
            while (!maxHeap.empty() && invalidCount[maxHeap.top()]) {
                invalidCount[maxHeap.top()]--;
                maxHeap.pop();
                maxHeapSize--;
            }
            while (!minHeap.empty() && invalidCount[minHeap.top()]) {
                invalidCount[minHeap.top()]--;
                minHeap.pop();
                minHeapSize--;
            }
        };

        int maxHeapSize = 0, minHeapSize = 0;

        for (int i = 0; i < nums.size(); i++) {
            addNum(nums[i]);
            if (i >= k - 1) { // The window is fully valid
                // Find the median
                if (maxHeapSize > minHeapSize) {
                    medians.push_back(maxHeap.top());
                } else {
                    medians.push_back(((double)maxHeap.top() + minHeap.top()) / 2.0);
                }
                // Remove the element going out of the window
                removeNum(nums[i - k + 1]);
            }
        }

        return medians;
    }
};
```

#### Time Complexity
- Each insertion or removal operation into heaps is `O(log k)`.
  
**Overall**: `O(n log k)`

#### Space Complexity
- Storing the elements in two heaps uses `O(k)` space. Also, an auxiliary map for invalid elements takes additional space.

---

This structured explanation provides insights into solving the "Sliding Window Median" problem, progressing from a naive sorting-based strategy to a more optimal heap-based approach.

