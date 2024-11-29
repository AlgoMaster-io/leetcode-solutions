# 973. [K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)

## Approach 1: Max-Heap

### Solution
```cpp
// Time Complexity: O(n * log(k)), where n is the number of points
// Space Complexity: O(k), for the max-heap
#include <vector>
#include <queue>
#include <cmath>

class Solution {
public:
    std::vector<std::vector<int>> kClosest(std::vector<std::vector<int>>& points, int k) {
        // Max-Heap to store the k closest points
        auto compare = [](std::vector<int>& a, std::vector<int>& b) {
            return distance(b) > distance(a);
        };
        std::priority_queue<std::vector<int>, std::vector<std::vector<int>>, decltype(compare)> maxHeap(compare);

        // Add points to the heap and maintain only the k closest points
        for (auto& point : points) {
            maxHeap.push(point);
            if (maxHeap.size() > k) {
                maxHeap.pop(); // Remove the farthest point
            }
        }

        // Extract the k closest points from the heap
        std::vector<std::vector<int>> result(k);
        for (int i = 0; i < k; i++) {
            result[i] = maxHeap.top();
            maxHeap.pop();
        }

        return result;
    }

private:
    static int distance(const std::vector<int>& point) {
        return point[0] * point[0] + point[1] * point[1];
    }
};
```

## Approach 2: Quickselect

### Solution
```cpp
// Time Complexity: O(n) on average, O(n^2) in the worst case
// Space Complexity: O(1), as it modifies the input array in-place
#include <vector>
#include <algorithm>

class Solution {
public:
    std::vector<std::vector<int>> kClosest(std::vector<std::vector<int>>& points, int k) {
        quickSelect(points, 0, points.size() - 1, k);
        return std::vector<std::vector<int>>(points.begin(), points.begin() + k);
    }

private:
    void quickSelect(std::vector<std::vector<int>>& points, int left, int right, int k) {
        if (left >= right) return;

        int pivotIndex = partition(points, left, right);
        if (pivotIndex == k) {
            return;
        } else if (pivotIndex < k) {
            quickSelect(points, pivotIndex + 1, right, k);
        } else {
            quickSelect(points, left, pivotIndex - 1, k);
        }
    }

    int partition(std::vector<std::vector<int>>& points, int left, int right) {
        std::vector<int> pivot = points[right];
        int pivotDistance = distance(pivot);
        int i = left;

        for (int j = left; j < right; j++) {
            if (distance(points[j]) < pivotDistance) {
                std::swap(points[i], points[j]);
                i++;
            }
        }
        std::swap(points[i], points[right]);
        return i;
    }

    int distance(const std::vector<int>& point) {
        return point[0] * point[0] + point[1] * point[1];
    }
};
```

