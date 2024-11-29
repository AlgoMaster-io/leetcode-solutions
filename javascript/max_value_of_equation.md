# [1499. Max Value of Equation](https://leetcode.com/problems/max-value-of-equation/)

## Approach 1: Using a Priority Queue (Heap-Based)

### Solution
```javascript
// Time Complexity: O(n log n)
// Space Complexity: O(n)
class Solution {
    findMaxValueOfEquation(points, k) {
        const maxHeap = []; // Max-heap: {y - x, x}
        let maxValue = Number.MIN_SAFE_INTEGER;

        for (const [x, y] of points) {
            // Remove points outside the range of k
            while (maxHeap.length > 0 && x - maxHeap[0][1] > k) {
                maxHeap.shift();
            }

            // If the heap is not empty, calculate the current value of the equation
            if (maxHeap.length > 0) {
                maxValue = Math.max(maxValue, maxHeap[0][0] + y + x);
            }

            // Add the current point to the heap
            maxHeap.push([y - x, x]);
            maxHeap.sort((a, b) => b[0] - a[0]); // Maintain max-heap property
        }

        return maxValue;
    }
}
```

## Approach 2: Using a Deque (Optimal Solution)

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
    findMaxValueOfEquation(points, k) {
        const deque = []; // Stores pairs {y - x, x}
        let maxValue = Number.MIN_SAFE_INTEGER;

        for (const [x, y] of points) {
            // Remove points outside the range of k
            while (deque.length > 0 && x - deque[0][1] > k) {
                deque.shift();
            }

            // If the deque is not empty, calculate the current value of the equation
            if (deque.length > 0) {
                maxValue = Math.max(maxValue, deque[0][0] + y + x);
            }

            // Remove points from the back of the deque that have a smaller y - x value
            while (deque.length > 0 && deque[deque.length - 1][0] <= y - x) {
                deque.pop();
            }

            // Add the current point to the deque
            deque.push([y - x, x]);
        }

        return maxValue;
    }
}
```

