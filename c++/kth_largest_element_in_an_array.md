# 215. [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)

## Approach 1: Sorting

### Solution

cpp
```cpp
// Time Complexity: O(n log n)
// Space Complexity: O(1)
#include <algorithm>
#include <vector>

class Solution {
public:
    int findKthLargest(std::vector<int>& nums, int k) {
        std::sort(nums.begin(), nums.end()); // Sort the array in ascending order
        return nums[nums.size() - k]; // Return the kth largest element
    }
};
```

## Approach 2: Using a Min-Heap

### Solution

cpp
```cpp
// Time Complexity: O(n log k)
// Space Complexity: O(k)
#include <queue>
#include <vector>

class Solution {
public:
    int findKthLargest(std::vector<int>& nums, int k) {
        std::priority_queue<int, std::vector<int>, std::greater<int>> minHeap; // Min-heap to keep the top k elements

        for (int num : nums) {
            minHeap.push(num); // Add the current number to the heap
            if (minHeap.size() > k) {
                minHeap.pop(); // Remove the smallest element if the heap size exceeds k
            }
        }

        return minHeap.top(); // The root of the heap is the kth largest element
    }
};
```

## Approach 3: Quickselect (Partitioning)

### Solution

cpp
```cpp
// Time Complexity: O(n) on average, O(n^2) in the worst case
// Space Complexity: O(1)
#include <vector>
#include <cstdlib>

class Solution {
public:
    int findKthLargest(std::vector<int>& nums, int k) {
        int n = nums.size();
        return quickSelect(nums, 0, n - 1, n - k);
    }

private:
    int quickSelect(std::vector<int>& nums, int left, int right, int k) {
        if (left == right) {
            return nums[left]; // Base case: only one element
        }

        int pivotIndex = left + rand() % (right - left + 1);
        pivotIndex = partition(nums, left, right, pivotIndex);

        if (pivotIndex == k) {
            return nums[k]; // Found the kth largest element
        } else if (pivotIndex < k) {
            return quickSelect(nums, pivotIndex + 1, right, k); // Search in the right part
        } else {
            return quickSelect(nums, left, pivotIndex - 1, k); // Search in the left part
        }
    }

    int partition(std::vector<int>& nums, int left, int right, int pivotIndex) {
        int pivotValue = nums[pivotIndex];
        std::swap(nums[pivotIndex], nums[right]); // Move pivot to the end
        int storeIndex = left;

        for (int i = left; i < right; i++) {
            if (nums[i] < pivotValue) {
                std::swap(nums[storeIndex], nums[i]);
                storeIndex++;
            }
        }

        std::swap(nums[storeIndex], nums[right]); // Move pivot to its final position
        return storeIndex;
    }
};
```

