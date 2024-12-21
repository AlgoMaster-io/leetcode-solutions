# [LeetCode 703: Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/)

## Approaches:
1. [Naive Approach: Sorting Each Time](#approach-1)
2. [Optimized Approach with Min-Heap](#approach-2)

---

## Approach 1: Naive Approach: Sorting Each Time

### Intuition
For each addition of an element, we can add it to our list and then sort the list in descending order. After which, the k-th largest element will simply be at the k-1 position. This approach, while straightforward, is quite inefficient because sorting the list for each new element addition is time-consuming.

### Code
```cpp
#include <vector>
#include <algorithm>

class KthLargest {
public:
    KthLargest(int k, std::vector<int>& nums) : k(k), nums(nums) {
        std::sort(this->nums.begin(), this->nums.end(), std::greater<int>());
    }
    
    int add(int val) {
        nums.push_back(val);
        std::sort(nums.begin(), nums.end(), std::greater<int>());
        return nums[k - 1];  // Return the k-th largest element
    }

private:
    int k;
    std::vector<int> nums;
};
```

### Complexity Analysis
- **Time Complexity:** `O(N log N)` per addition, due to sorting N elements (where N is the size of the array). Initialization requires `O(N log N)` as well for the initial sort.
- **Space Complexity:** `O(N)`, where N is the number of elements in the list.

---

## Approach 2: Optimized Approach with Min-Heap

### Intuition
To optimize, we can make use of a min-heap (also known as a priority queue). The key insight here is that, in order to determine the k-th largest element, we only need to keep track of the k largest elements seen so far. A min-heap of size k will hold these top k elements. The root of this min-heap will be the k-th largest element. When we add a new element, if it's larger than the smallest element in our heap, we replace the root with the new element and adjust the heap.

### Code
```cpp
#include <vector>
#include <queue>

class KthLargest {
public:
    KthLargest(int k, std::vector<int>& nums) : k(k) {
        for (int num : nums) {
            add(num);
        }
    }
    
    int add(int val) {
        if (minHeap.size() < k) {
            // If heap size is less than k, simply add the element.
            minHeap.push(val);
        } else if (val > minHeap.top()) {
            // If the new element is larger than the smallest element, replace it.
            minHeap.pop();
            minHeap.push(val);
        }
        // The k-th largest element will be at the top of the min-heap.
        return minHeap.top();
    }

private:
    int k;
    std::priority_queue<int, std::vector<int>, std::greater<int>> minHeap;
};
```

### Complexity Analysis
- **Time Complexity:** `O(log K)` per addition. The operations on a heap are logarithmic with respect to its size. Initialization takes `O(N log K)` as we build our heap from an initial list of N elements.
- **Space Complexity:** `O(K)`, where K is the number of elements that the min-heap stores.

