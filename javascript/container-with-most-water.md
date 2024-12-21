# [Leetcode 11: Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

## Approaches:
1. [Brute Force](#brute-force)
2. [Two-pointer Technique](#two-pointer-technique)

### Brute Force

The brute force approach looks at every possible pair of lines to determine the maximum water they can trap. This method involves nested loops and is straightforward to implement, although not efficient.

#### Intuition:
1. For each pair of lines (`i, j`), calculate the area the water can trap as the minimum of the heights of the two lines multiplied by the distance between them.
2. Update the maximum area if the current area is larger.

#### Code:

```javascript
function maxArea(height) {
    let maxArea = 0;
    // Iterate through each pair of lines
    for (let i = 0; i < height.length; i++) {
        for (let j = i + 1; j < height.length; j++) {
            // Calculate the area for the current pair of lines
            const currentArea = Math.min(height[i], height[j]) * (j - i);
            // Update the maximum area if needed
            maxArea = Math.max(maxArea, currentArea);
        }
    }
    return maxArea;
}
```

#### Time Complexity:
- **O(n^2)**: We have a nested loop iterating over `n*(n-1)/2` possible pairs of lines.  
#### Space Complexity:
- **O(1)**: No additional space is used beyond the few variables.

### Two-pointer Technique

The two-pointer technique improves efficiency by exploiting the fact that we can maximize the area by moving pointers from both ends towards the center.

#### Intuition:
1. Start with two pointers, one at the beginning (`left`) and one at the end (`right`) of the height array.
2. Calculate the area between the two lines at the `left` and `right` pointers.
3. Move the pointer pointing to the shorter line towards the other pointer since moving the taller pointer will not help in finding a longer container.
4. Repeat the above steps until the `left` and `right` pointers meet.

#### Code:

```javascript
function maxArea(height) {
    let maxArea = 0;
    let left = 0; // Start pointer from the beginning
    let right = height.length - 1; // Start pointer from the end

    // Traverse until the two pointers meet
    while (left < right) {
        // Calculate the area using the shorter line since water cannot go above it
        const currentArea = Math.min(height[left], height[right]) * (right - left);
        maxArea = Math.max(maxArea, currentArea);

        // Move the pointer pointing to the shorter line
        if (height[left] < height[right]) {
            left++;
        } else {
            right--;
        }
    }
    return maxArea;
}
```

#### Time Complexity:
- **O(n)**: Each line is considered once, with the `left` and `right` pointers moving towards each other.
#### Space Complexity:
- **O(1)**: No additional space is used beyond the variables for pointers and max area.

This two-pointer approach dramatically reduces the number of iterations needed to find the maximum possible area, making it the optimal solution for this problem.

