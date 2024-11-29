# 295. [Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)

## Approach: Two Heaps

### Solution
```cpp
// Time Complexity: O(log(n)) for adding a number, O(1) for finding the median
// Space Complexity: O(n), where n is the number of elements in the data stream
#include <queue>
#include <functional>

class MedianFinder {
private:
    std::priority_queue<int> maxHeap; // To store the smaller half of the numbers
    std::priority_queue<int, std::vector<int>, std::greater<int>> minHeap; // To store the larger half of the numbers

public:
    MedianFinder() {
        // The heaps are initialized implicitly
    }

    void addNum(int num) {
        // Add to maxHeap, then balance by adding the largest of maxHeap to minHeap
        maxHeap.push(num);
        minHeap.push(maxHeap.top());
        maxHeap.pop();

        // Ensure maxHeap always has the same number or one more element than minHeap
        if (maxHeap.size() < minHeap.size()) {
            maxHeap.push(minHeap.top());
            minHeap.pop();
        }
    }

    double findMedian() {
        if (maxHeap.size() > minHeap.size()) {
            return maxHeap.top(); // Odd number of elements
        }
        return (maxHeap.top() + minHeap.top()) / 2.0; // Even number of elements
    }
};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder* obj = new MedianFinder();
 * obj->addNum(num);
 * double param_2 = obj->findMedian();
 */
```

