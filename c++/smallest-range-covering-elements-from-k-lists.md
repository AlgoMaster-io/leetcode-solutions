# [Leetcode 632: Smallest Range Covering Elements from K Lists](https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/)

In this problem, we are given `k` sorted integer lists and our goal is to find the smallest range that includes at least one number from each of the `k` lists.

### Approach Navigation
- [Approach 1: Priority Queue (Min-Heap)](#approach-1-priority-queue-min-heap)

---

## Approach 1: Priority Queue (Min-Heap)
The problem can be tackled efficiently using a priority queue (or min-heap). The idea is to maintain the smallest element that can be removed from consideration while ensuring one element from each list is always included in the current window.

### Intuition
The key observation is that since each list is sorted, the smallest possible range can be constructed by checking valid windows that start with the smallest elements from each list. At each step, we update our range and try to grow this window by including the next element from the list of the current smallest element.

We use a priority queue to efficiently fetch the current smallest element across all k lists.

### Steps
1. Insert the first element of each list into a priority queue. Keep track of the current maximum value among these first elements.
2. While the priority queue is not empty:
   - Extract the minimum element from the priority queue.
   - Check the current range `[min, max]` and update the smallest range found.
   - If the extracted element's index within its list allows, push the next element from that list into the priority queue, updating the current maximum if needed.
   - If we've pushed elements from some list till the end, break the loop since you're unable to create a valid range without an element from all lists.

### Detailed Comments in Code

```cpp
#include <vector>
#include <queue>
#include <limits.h>
using namespace std;

// Define a triple structure to represent value and its source in pq
struct Node {
    int value;
    int row; // row represents from which list the value is taken
    int idx; // index in that particular list
    bool operator>(const Node& other) const {
        return value > other.value;
    }
};

class Solution {
public:
    vector<int> smallestRange(vector<vector<int>>& nums) {
        int minRange = INT_MAX;
        int currentMax = INT_MIN; // Max element in current window

        // Priority queue to store elements by value
        priority_queue<Node, vector<Node>, greater<Node>> pq;

        // Initialization: Insert the first element of each list
        for (int i = 0; i < nums.size(); ++i) {
            pq.push({nums[i][0], i, 0});
            currentMax = max(currentMax, nums[i][0]); // Track the maximum
        }
        
        int start = 0, end = 0;

        // While possible to maintain an element from each list
        while (!pq.empty()) {
            Node curr = pq.top(); // Min element from queue
            pq.pop();

            // Calculate current range
            int currentRange = currentMax - curr.value;
            if (currentRange < minRange) {
                minRange = currentRange;
                start = curr.value; // New range start
                end = currentMax;   // New range end
            }

            // Move to the next element in the same list
            if (curr.idx + 1 < nums[curr.row].size()) {
                int nextValue = nums[curr.row][curr.idx + 1];
                pq.push({nextValue, curr.row, curr.idx + 1});
                currentMax = max(currentMax, nextValue); // Update current max
            } else {
                // If reached the end of the list, break loop
                break;
            }
        }

        return {start, end};
    }
};
```

### Time Complexity
- The time complexity is \(O(N \log K)\) where \(N\) is the total number of elements across all lists and \(K\) is the number of lists. The heap operations take \(\log K\) time and we perform them for each element.

### Space Complexity
- The space complexity is \(O(K)\), predominantly due to the space required for the priority queue, at most storing one element per list at a time.

