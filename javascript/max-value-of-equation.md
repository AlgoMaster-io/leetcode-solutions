# [Leetcode 1499: Max Value of Equation](https://leetcode.com/problems/max-value-of-equation/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Optimal Approach Using a Max-Heap](#optimal-approach-using-a-max-heap)

---

## Brute Force Approach

### Intuition
The problem requires us to find a pair of points that maximizes the given equation while ensuring they satisfy a specific constraint on their x-coordinates. The most straightforward method is to examine each possible pair of points and calculate the equation value for valid pairs.

For each pair of points (i, j) where `i < j` and `|xi - xj| <= k`, we calculate the value of the equation `yi + yj + |xi - xj|` using the formula `yi + yj + (xj - xi)`. Loop through every possible pair and keep track of the maximum value found that satisfies the condition.

### Code
```javascript
var findMaxValueOfEquation = function(points, k) {
    let maxValue = -Infinity; // Initialize the maximum value as negative infinity
    const n = points.length;

    // Loop through each pair of points
    for (let i = 0; i < n; i++) {
        for (let j = i + 1; j < n; j++) {
            const [x1, y1] = points[i];
            const [x2, y2] = points[j];

            // Check if the pair of points satisfies the |xj - xi| <= k condition
            if (x2 - x1 <= k) {
                // Calculate the equation value for this pair
                const value = y1 + y2 + (x2 - x1);
                // Update the maxValue if we find a larger value
                maxValue = Math.max(maxValue, value);
            }
        }
    }

    return maxValue;
};

```

### Time Complexity
- **O(n²)**: Iterates over every pair of points leading to quadratic time complexity.

### Space Complexity
- **O(1)**: No extra data structures are used, just a variable for tracking the maximum value.

---

## Optimal Approach Using a Max-Heap

### Intuition
To make the solution more efficient, we leverage the idea of using a max-heap to store values and their positions that can potentially form valid pairs. The max-heap will help to track the maximum possible value for `yi - xi` encountered so far, which will be beneficial since the equation can be rewritten as `yi + yj + (xj - xi) = (yi - xi) + (yj + xj)`.

For each point `j`, we can calculate `yj + xj`, and if there exists `i` such that `|xi - xj| <= k`, we want to add the maximum `(yi - xi)` from previous points to this value.

### Code
```javascript
var findMaxValueOfEquation = function(points, k) {
    const maxHeap = []; // Use a max-heap to keep track of maximum yi - xi
    let maxValue = -Infinity;

    for (let [xj, yj] of points) {
        // Remove elements from heap that do not satisfy xj - xi <= k
        while (maxHeap.length && Math.abs(maxHeap[0][1] - xj) > k) {
            maxHeap.shift();
        }

        // Calculate potential max value using current point and maxHeap's top element
        if (maxHeap.length) {
            maxValue = Math.max(maxValue, yj + xj + maxHeap[0][0]);
        }

        // Insert current (yi - xi, xi) into heap
        const valuePair = [yj - xj, xj];
        maxHeap.push(valuePair);

        // Sort the heap based on the value of yi - xi
        maxHeap.sort((a, b) => b[0] - a[0]);
    }

    return maxValue;
};
```

### Time Complexity
- **O(n \* log n)**: Each operation involving the heap is logarithmic, and each input point leads to one such operation.

### Space Complexity
- **O(n)**: The auxiliary space for the heap.

The optimal approach employs a max-heap and efficiently narrows down possible pairs by maintaining constraints, reducing unnecessary computations significantly.

