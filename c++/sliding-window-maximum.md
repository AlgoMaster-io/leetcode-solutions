# [Leetcode 239: Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

### Approaches
- [Naive Approach using Two Nested Loops](#naive-approach-using-two-nested-loops)
- [Optimized Approach using a Deque](#optimized-approach-using-a-deque)

## Naive Approach using Two Nested Loops

### Intuition
The simplest way to solve this problem is to calculate the maximum of each window using a nested loop. For each starting position of the window, iterate through the next `k` elements to find the maximum.

### Approach
- Iterate over the array, setting the window's starting position.
- For each starting position, iterate through the next `k` elements to find the maximum.
- Store each maximum value in a results array.

### Code
```cpp
#include <vector>
#include <climits>

std::vector<int> maxSlidingWindow(std::vector<int>& nums, int k) {
    std::vector<int> result;
    for (int i = 0; i <= nums.size() - k; ++i) {
        int max_val = INT_MIN;
        // Check all elements in the current window
        for (int j = i; j < i + k; ++j) {
            if (nums[j] > max_val) {
                max_val = nums[j];
            }
        }
        result.push_back(max_val);
    }
    return result;
}
```

### Time and Space Complexity
- **Time Complexity**: \(O(nk)\), where \(n\) is the number of elements in the list. For each of the \(n-k+1\) windows, a linear scan over \(k\) elements is performed.
- **Space Complexity**: \(O(1)\), as no additional space proportional to input size is used, other than the result list.

## Optimized Approach using a Deque

### Intuition
A more efficient way leverages a deque to keep track of relevant indices in the current window. The deque helps to quickly find the current maximum without checking all elements in the window each time.

### Approach
- Use a deque to store indices of array elements. The deque is maintained such that the elements corresponding to these indices are in decreasing order.
- For each element in nums:
  - Remove indices that are out of the current window.
  - Remove elements from the back of the deque while they are smaller than the current element.
  - Add the current element index to the deque.
  - Save the largest element for each valid window by checking the front of the deque.

### Code
```cpp
#include <vector>
#include <deque>

std::vector<int> maxSlidingWindow(std::vector<int>& nums, int k) {
    std::deque<int> deq;
    std::vector<int> result;
    for (int i = 0; i < nums.size(); ++i) {
        // Remove indices that are out of the current window
        if (!deq.empty() && deq.front() == i - k) {
            deq.pop_front();
        }
        // Remove elements in deque that are less than the current element
        while (!deq.empty() && nums[deq.back()] < nums[i]) {
            deq.pop_back();
        }
        // Add current element to the deque
        deq.push_back(i);
        
        // The front of the deque is the largest element of the window
        if (i >= k - 1) {
            result.push_back(nums[deq.front()]);
        }
    }
    return result;
}
```

### Time and Space Complexity
- **Time Complexity**: \(O(n)\), each element is inserted and removed from the deque exactly once.
- **Space Complexity**: \(O(k)\), as the deque stores at most \(k\) elements at any time.

