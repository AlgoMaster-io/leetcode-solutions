# [42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

## Approach 1: Brute Force (Basic)

### Solution
```javascript
// Time Complexity: O(n^2)
// Space Complexity: O(1)
var trap = function(height) {
    let n = height.length;
    let totalWater = 0;

    // Iterate over each bar
    for (let i = 1; i < n - 1; i++) {
        let leftMax = 0;
        let rightMax = 0;

        // Find the maximum height to the left of the current bar
        for (let j = 0; j <= i; j++) {
            leftMax = Math.max(leftMax, height[j]);
        }

        // Find the maximum height to the right of the current bar
        for (let j = i; j < n; j++) {
            rightMax = Math.max(rightMax, height[j]);
        }

        // Calculate water trapped at the current bar
        totalWater += Math.min(leftMax, rightMax) - height[i];
    }

    return totalWater;
};
```

## Approach 2: Dynamic Programming

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(n)
var trap = function(height) {
    let n = height.length;
    if (n === 0) return 0;

    let leftMax = new Array(n);
    let rightMax = new Array(n);
    let totalWater = 0;

    // Compute leftMax array
    leftMax[0] = height[0];
    for (let i = 1; i < n; i++) {
        leftMax[i] = Math.max(leftMax[i - 1], height[i]);
    }

    // Compute rightMax array
    rightMax[n - 1] = height[n - 1];
    for (let i = n - 2; i >= 0; i--) {
        rightMax[i] = Math.max(rightMax[i + 1], height[i]);
    }

    // Calculate water trapped at each bar
    for (let i = 0; i < n; i++) {
        totalWater += Math.min(leftMax[i], rightMax[i]) - height[i];
    }

    return totalWater;
};
```

## Approach 3: Two Pointers (Optimal)

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(1)
var trap = function(height) {
    let left = 0, right = height.length - 1;
    let leftMax = 0, rightMax = 0;
    let totalWater = 0;

    // Use two pointers to calculate trapped water
    while (left < right) {
        if (height[left] < height[right]) {
            // Update leftMax and calculate water at left pointer
            if (height[left] >= leftMax) {
                leftMax = height[left];
            } else {
                totalWater += leftMax - height[left];
            }
            left++;
        } else {
            // Update rightMax and calculate water at right pointer
            if (height[right] >= rightMax) {
                rightMax = height[right];
            } else {
                totalWater += rightMax - height[right];
            }
            right--;
        }
    }

    return totalWater;
};
```

