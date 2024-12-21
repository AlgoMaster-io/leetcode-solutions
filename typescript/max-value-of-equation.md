# [Leetcode 1499: Max Value of Equation](https://leetcode.com/problems/max-value-of-equation/)

## Approaches

1. [Brute Force Approach](#brute-force-approach)
2. [Efficient Approach Using Monotonic Queue](#efficient-approach-using-monotonic-queue)

### Brute Force Approach

#### Intuition

The brute force method involves checking each pair of points `(x1, y1)` and `(x2, y2)` for the condition `x2 - x1 <= k`, and computing the value of the equation: `yi + yj + |xi - xj|`. By iterating through every possible pair, we can determine which one yields the maximum value. Although simple, this approach can be inefficient given its time complexity.

#### Steps in Code:

1. Iterate over all pairs `(x[i], y[i])` and `(x[j], y[j])`.
2. For each pair, check if the condition `x[j] - x[i] <= k` holds.
3. If true, compute the equation value and update the maximum if this value is greater than the previously recorded maximum.

#### Time and Space Complexity

- **Time complexity**: O(n^2) because every pair of points is checked.
- **Space complexity**: O(1) since no additional space is used beyond a few variables.

#### Typescript Implementation:

```typescript
function findMaxValueOfEquationBruteForce(points: number[][], k: number): number {
    let maxValue = -Infinity;
    const n = points.length;
    
    for (let i = 0; i < n; i++) {
        for (let j = i + 1; j < n; j++) {
            const [x1, y1] = points[i];
            const [x2, y2] = points[j];
            
            if (x2 - x1 <= k) {
                // Compute value of the equation
                const value = y1 + y2 + (x2 - x1);
                // Update the max value if the current value is larger
                maxValue = Math.max(maxValue, value);
            }
        }
    }
    
    return maxValue;
}
```

### Efficient Approach Using Monotonic Queue

#### Intuition

To improve performance, we can utilize a deque (double-ended queue) that helps in maintaining potential candidates for maximum values, based on the recently passed points. By leveraging a deque, we keep only the points that are feasible contenders, ensuring operations remain efficient even with large input sizes.

1. The deque helps to quickly find the maximum value for `yi - xi` that meets the condition with an `xj - xi <= k`.
2. By maintaining the elements in a way that keeps the largest `yi - xi` at the front, we can efficiently compute the eventual result as we iterate through the points.

#### Steps in Code:

1. Initialize a deque to keep candidates points.
2. Iterate over each point `(x, y)`.
3. Before processing a new point, remove elements from the deque that do not satisfy `xj - xi <= k`.
4. Calculate the potential maximum using the front of the deque.
5. Add the current point to the deque. Ensure the deque maintains the property of having the maximum possible `yi - xi` at the front.

#### Time and Space Complexity

- **Time complexity**: O(n) - Each element is added and removed from the deque at most once.
- **Space complexity**: O(n) - Due to the deque possibly containing all points in the worst case.

#### Typescript Implementation:

```typescript
function findMaxValueOfEquationEfficient(points: number[][], k: number): number {
    let deque: [number, number][] = [];
    let maxVal = -Infinity;
        
    for (const [x, y] of points) {
        // Remove elements from the deque that are out of the current window
        while (deque.length && x - deque[0][1] > k) {
            deque.shift();
        }
        
        // Check the maximum value using the front element in the deque
        if (deque.length) {
            maxVal = Math.max(maxVal, y + x + deque[0][0]);
        }
        
        // Maintain the deque to have the largest yi - xi
        // Remove from the back if new element is better
        while (deque.length && deque[deque.length - 1][0] <= y - x) {
            deque.pop();
        }
        
        // Add current (y - x, x) to the deque
        deque.push([y - x, x]);
    }
    
    return maxVal;
}
```

Both solutions given above provide different approaches to solving the problem, with the second being more efficient for larger inputs.

