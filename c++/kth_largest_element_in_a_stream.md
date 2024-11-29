# 703. [Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/)

## Approach: Min-Heap

### Solution
```cpp
// Time Complexity: O(n * log(k)) for initialization and O(log(k)) for each add operation
// Space Complexity: O(k), where k is the size of the heap
#include <queue>
#include <vector>

class KthLargest {
public:
    KthLargest(int k, std::vector<int>& nums) {
        this->k = k;
        // Create a min-heap to store the k largest elements
        for (int num : nums) {
            add(num);
        }
    }
    
    int add(int val) {
        minHeap.push(val); // Add the new value to the heap

        // Remove the smallest element if the heap exceeds size k
        if (minHeap.size() > k) {
            minHeap.pop();
        }

        return minHeap.top(); // The kth largest element is the root of the heap
    }

private:
    std::priority_queue<int, std::vector<int>, std::greater<int>> minHeap; // min-heap to store the k largest elements
    int k;
};

/**
 * Your KthLargest object will be instantiated and called as such:
 * KthLargest* obj = new KthLargest(k, nums);
 * int param_1 = obj->add(val);
 */
```

