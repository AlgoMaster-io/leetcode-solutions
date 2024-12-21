## [Leetcode 295: Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)

### Approaches:
1. [Naive approach using sorting](#naive-approach-using-sorting)
2. [Two Heaps (Optimal Solution)](#two-heaps-optimal-solution)

---

### Naive approach using sorting

#### Intuition:
The simplest approach to find the median is to maintain a list of numbers and keep the list sorted each time we add a new number to it. Once sorted, finding the median is straightforward – it's the middle number if the count of numbers is odd, and the average of the two middle numbers if it's even.

#### Approach:
1. Maintain an array or vector of numbers.
2. Each time a new number is added, insert it into the vector.
3. Sort the vector each time a new number is added.
4. Compute the median:
   - If the size of the container is odd, median is the middle element.
   - If the size is even, median is the average of two middle elements.

#### Complexity Analysis:
- **Time Complexity:** `O(n log n)` for each insertion, as we sort the list.
- **Space Complexity:** `O(n)` to store the numbers.

```cpp
#include <vector>
#include <algorithm>

class MedianFinder {
private:
    std::vector<int> nums;  // Vector to store numbers

public:
    MedianFinder() {}

    void addNum(int num) {
        nums.push_back(num);  // Add new number
        std::sort(nums.begin(), nums.end());  // Sort the vector
    }

    double findMedian() {
        int n = nums.size();
        if (n % 2 == 1) {
            return nums[n / 2];  // Odd number of elements
        } else {
            return (nums[n / 2 - 1] + nums[n / 2]) / 2.0;  // Even number of elements
        }
    }
};
```

---

### Two Heaps (Optimal Solution)

#### Intuition:
To efficiently find the median in a data stream, we can use two heaps:
- A max heap for the lower half of numbers.
- A min heap for the upper half of numbers.
This setup allows us to access the median in constant time and adjust the heaps with logarithmic time complexity during insertions.

#### Approach:
1. Use a max heap to keep track of the smaller half (lower half) of the numbers.
2. Use a min heap to keep track of the larger half (upper half) of the numbers.
3. On insertion:
   - Insert into the max heap. Then, move the maximum from the max heap into the min heap.
   - If the size of the min heap exceeds the size of the max heap, move the minimum from the min heap back into the max heap.
4. To find the median:
   - If the total number of elements is odd, the max heap will have one extra element which will be the median.
   - If even, return the average of the tops of both heaps.

#### Complexity Analysis:
- **Time Complexity:** `O(log n)` for each insertion due to heap insertion operations.
- **Space Complexity:** `O(n)` to store the numbers in the heaps.

```cpp
#include <queue>
#include <functional>

class MedianFinder {
private:
    std::priority_queue<int> maxHeap; // Max heap for the lower half
    std::priority_queue<int, std::vector<int>, std::greater<int>> minHeap; // Min heap for the upper half

public:
    MedianFinder() {}

    void addNum(int num) {
        maxHeap.push(num);  // Add to max heap
        minHeap.push(maxHeap.top());  // Balance: move the largest from max heap to min heap
        maxHeap.pop();  // Remove from max heap since it moved to min heap

        // Ensure size property
        if (minHeap.size() > maxHeap.size()) {
            maxHeap.push(minHeap.top());  // Move the smallest from min heap back to max heap
            minHeap.pop();
        }
    }

    double findMedian() {
        if (maxHeap.size() > minHeap.size()) {
            return maxHeap.top();  // Odd case
        } else {
            return (maxHeap.top() + minHeap.top()) / 2.0;  // Even case
        }
    }
};
```

This solution efficiently supports real-time data streams and consistently provides the median with a balanced and minimal overhead.

