# [Leetcode 1499: Max Value of Equation](https://leetcode.com/problems/max-value-of-equation/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Optimized using Priority Queue (Max-Heap)](#approach-2-optimized-using-priority-queue-max-heap)

## Approach 1: Brute Force

### Intuition
The problem asks to find the maximum value of the expression: `yi + yj + |xi - xj|` for all i < j and where |xi - xj| <= k. We can achieve a solution using a brute force method by iterating over all pairs of points and checking for the condition `|xi - xj| <= k`. We then compute the value of the expression and keep track of the maximum value found.

### Steps
1. Iterate over all pairs of points `(xi, yi)` and `(xj, yj)`.
2. For each pair, check if `|xi - xj| <= k`.
3. Compute the expression `yi + yj + (xj - xi)` if the condition is satisfied.
4. Keep track of the maximum value obtained from all valid pairs.

### Complexity
- **Time Complexity**: O(n^2) where n is the number of points. This is because we are checking all possible pairs.
- **Space Complexity**: O(1) additional space since we only need variables to store maximum value.

```cpp
#include <vector>
#include <cmath>
#include <climits>

class Solution {
public:
    int findMaxValueOfEquation(std::vector<std::vector<int>>& points, int k) {
        int maxValue = INT_MIN;
        int n = points.size();
        
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                // Check if the condition |xi - xj| <= k holds
                if (std::abs(points[i][0] - points[j][0]) <= k) {
                    // Calculate the value of the equation for the pair
                    int currentValue = points[i][1] + points[j][1] + std::abs(points[j][0] - points[i][0]);
                    maxValue = std::max(maxValue, currentValue);
                }
            }
        }
        
        return maxValue;
    }
};
```

## Approach 2: Optimized using Priority Queue (Max-Heap)

### Intuition
The brute force approach is not efficient due to O(n^2) complexity. To improve this, note that the expression can be restructured as `yj + xj + (yi - xi)`. We can store values of `(yi - xi)` in a max-heap (priority queue) since we want the maximum of this value as `xj` increases and maintain the constraint `xj - xi <= k`.

### Steps
1. Utilize a max-heap (priority queue) to store pairs of `(yi - xi, xi)`.
2. For each point `(xj, yj)`, remove elements from the heap that do not satisfy the constraint `xj - xi <= k`.
3. Calculate the maximal value using the remaining suitable pair in the heap.
4. Push the current pair `(yj - xj, xj)` into the heap.
5. Continue the process and update the maximum value found.

### Complexity
- **Time Complexity**: O(n log n) where n is the number of points, due to the operations on the priority queue.
- **Space Complexity**: O(n) for the priority queue to store elements.

```cpp
#include <vector>
#include <queue>
#include <climits>

class Solution {
public:
    int findMaxValueOfEquation(std::vector<std::vector<int>>& points, int k) {
        // Using a max-heap to keep track of the largest (yi - xi)
        std::priority_queue<std::pair<int, int>> maxHeap;
        int maxValue = INT_MIN;
        
        // Iterate through all given points
        for (const auto& point : points) {
            int xj = point[0], yj = point[1];

            // Remove elements from the heap that violate the condition xj - xi <= k
            while (!maxHeap.empty() && (xj - maxHeap.top().second > k)) {
                maxHeap.pop();
            }

            // If the heap is not empty, calculate the potential max value
            if (!maxHeap.empty()) {
                int potentialMax = yj + xj + maxHeap.top().first;
                maxValue = std::max(maxValue, potentialMax);
            }

            // Push the current point's (yi - xi, xi) into the heap
            maxHeap.emplace(yj - xj, xj);
        }
        
        return maxValue;
    }
};
```

This optimized solution efficiently handles larger input sizes by leveraging the properties of the max-heap to maintain worthwhile candidate pairs efficiently.

