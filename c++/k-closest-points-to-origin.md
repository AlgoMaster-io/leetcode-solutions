# [Leetcode 973: K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)

## Approaches:
- [Approach 1: Sorting](#approach-1-sorting)
- [Approach 2: Max-Heap (Priority Queue)](#approach-2-max-heap-priority-queue)
- [Approach 3: QuickSelect (QuickSort Partition)](#approach-3-quickselect-quicksort-partition)

## Approach 1: Sorting

### Intuition
The first approach is straightforward. Calculate the Euclidean distance for each point from the origin. Then, sort the points based on their distances and pick the first \(K\) points. This approach is simple as it leverages sorting, which is already a well-known problem.

### Code
```cpp
#include <vector>
#include <algorithm>
#include <cmath>

class Solution {
public:
    vector<vector<int>> kClosest(vector<vector<int>>& points, int K) {
        // Sort the points based on the Euclidean distance
        sort(points.begin(), points.end(), [](const vector<int>& a, const vector<int>& b) {
            return (a[0] * a[0] + a[1] * a[1]) < (b[0] * b[0] + b[1] * b[1]);
        });
        
        // Return the first K points
        return vector<vector<int>>(points.begin(), points.begin() + K);
    }
};
```

### Time Complexity
- Sorting the list takes \(O(N \log N)\) where \(N\) is the number of points.
- Thus, the overall complexity is \(O(N \log N)\).

### Space Complexity
- This approach sorts the points in place, hence it requires \(O(1)\) additional space.

## Approach 2: Max-Heap (Priority Queue)

### Intuition
Instead of sorting all points, we can maintain a max-heap of size \(K\) to keep track of the \(K\) smallest distances found so far. This way, we only perform heavy operations on a smaller subset of the points.

### Code
```cpp
#include <vector>
#include <queue>
#include <cmath>

class Solution {
public:
    vector<vector<int>> kClosest(vector<vector<int>>& points, int K) {
        // Max-heap to maintain the K closest points
        auto comparer = [](const vector<int>& a, const vector<int>& b) {
            return (a[0] * a[0] + a[1] * a[1]) < (b[0] * b[0] + b[1] * b[1]);
        };
        
        priority_queue<vector<int>, vector<vector<int>>, decltype(comparer)> max_heap(comparer);
        
        // Iterate through each point 
        for (const auto& point : points) {
            // Push current point onto heap
            max_heap.push(point);
            // If heap size exceeds K, pop the farthest element
            if (max_heap.size() > K) {
                max_heap.pop();
            }
        }
        
        // Gather the K closest points from the heap
        vector<vector<int>> result;
        while (!max_heap.empty()) {
            result.push_back(max_heap.top());
            max_heap.pop();
        }
        
        return result;
    }
};
```

### Time Complexity
- Each insertion and deletion from the heap takes \(O(\log K)\).
- We process all \(N\) points, hence the overall time complexity is \(O(N \log K)\).

### Space Complexity
- The space complexity is \(O(K)\) for the heap.

## Approach 3: QuickSelect (QuickSort Partition)

### Intuition
QuickSelect is a selection algorithm for finding the k-th smallest element in an unordered list. It uses a similar approach as QuickSort. The idea is to perform partitioning and recursively decide which partition to discard, leading to finding the correct \(K\) elements.

### Code
```cpp
#include <vector>
#include <algorithm>
#include <utility>

class Solution {
public:
    vector<vector<int>> kClosest(vector<vector<int>>& points, int K) {
        // Custom partition to organize points
        auto partition = [&points](int left, int right) {
            int pivot_distance = points[right][0] * points[right][0] + points[right][1] * points[right][1];
            int store_index = left;
            for (int i = left; i < right; ++i) {
                if ((points[i][0] * points[i][0] + points[i][1] * points[i][1]) <= pivot_distance) {
                    std::swap(points[i], points[store_index]);
                    ++store_index;
                }
            }
            std::swap(points[store_index], points[right]);
            return store_index;
        };

        // QuickSelect main operation
        int left = 0, right = points.size() - 1;
        while (left <= right) {
            int pivot_index = partition(left, right);
            if (pivot_index == K - 1) {
                break;
            }
            if (pivot_index < K - 1) {
                left = pivot_index + 1;
            } else {
                right = pivot_index - 1;
            }
        }
        
        // Slice the first K elements
        return vector<vector<int>>(points.begin(), points.begin() + K);
    }
};
```

### Time Complexity
- The average case time complexity of QuickSelect is \(O(N)\).
- The worst-case time complexity is \(O(N^2)\), though with good pivot selection strategies, this is rare.

### Space Complexity
- The space complexity is \(O(1)\) as it modifies the array in place.

Each approach offers its own trade-offs. Using sorting is the simplest and involves existing library functions, whereas the heap and quickselect methods can be more efficient for larger \(K\) values, though they involve more complexity in terms of implementation.

