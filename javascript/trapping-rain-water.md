# [Leetcode 42: Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Dynamic Programming Approach](#dynamic-programming-approach)
3. [Two Pointers Approach](#two-pointers-approach)
4. [Using a Monotonic Stack](#using-a-monotonic-stack)

### Brute Force Approach

In the brute force method, we consider each element as a potential candidate to trap water. The water that can be trapped above a bar is determined by the height of the smallest of two highest bars on both sides minus the height of the current bar.

#### Intuition:
For each element in the array, calculate the maximum height on its left and the maximum height on its right. The amount of water that can be trapped above this element is given by the minimum of these two heights minus the height of the element itself.

#### Time Complexity:
- O(n^2), where n is the number of elements in the array because for each element, we are calculating the left and right max elements.

#### Space Complexity:
- O(1), because we are using constant extra space.

```javascript
function trap(height) {
    let waterTrapped = 0;
    for (let i = 0; i < height.length; i++) {
        // Find the maximum height to the left of the current element
        let leftMax = 0;
        for (let j = 0; j <= i; j++) {
            leftMax = Math.max(leftMax, height[j]);
        }  
        // Find the maximum height to the right of the current element
        let rightMax = 0;
        for (let j = i; j < height.length; j++) {
            rightMax = Math.max(rightMax, height[j]);
        }
        // Water trapped at the current element
        waterTrapped += Math.min(leftMax, rightMax) - height[i];
    }
    return waterTrapped;
}
```

### Dynamic Programming Approach

To optimize the computation of left and right maximums at each step, we precompute these values and store them in arrays.

#### Intuition:
Instead of recalculating the left and right maximums for each bar, store them in arrays (`leftMax` and `rightMax`) to save on recalculations.

#### Time Complexity:
- O(n), where n is the number of elements in the array.

#### Space Complexity:
- O(n), due to the storage of two arrays to store left and right maximum heights.

```javascript
function trap(height) {
    if (height == null || height.length === 0) return 0;

    let waterTrapped = 0;
    const leftMax = [];
    const rightMax = [];

    leftMax[0] = height[0];
    for (let i = 1; i < height.length; i++) {
        leftMax[i] = Math.max(height[i], leftMax[i - 1]);
    }

    rightMax[height.length - 1] = height[height.length - 1];
    for (let i = height.length - 2; i >= 0; i--) {
        rightMax[i] = Math.max(height[i], rightMax[i + 1]);
    }

    for (let i = 0; i < height.length; i++) {
        waterTrapped += Math.min(leftMax[i], rightMax[i]) - height[i];
    }
    return waterTrapped;
}
```

### Two Pointers Approach

This method uses two pointers to keep track of the maximum seen so far from the left and right simultaneously.

#### Intuition:
Use two pointers, one starting from the beginning (`left`) and the other from the end (`right`). Compare the heights at these pointers and move the pointer with the smaller height inward. Calculate water trapped at this position.

#### Time Complexity:
- O(n), where n is the number of elements in the array.

#### Space Complexity:
- O(1), as no additional data structures are used.

```javascript
function trap(height) {
    if (height == null || height.length === 0) return 0;

    let left = 0, right = height.length - 1;
    let leftMax = 0, rightMax = 0, waterTrapped = 0;

    while (left < right) {
        if (height[left] < height[right]) {
            if (height[left] >= leftMax) {
                leftMax = height[left];
            } else {
                waterTrapped += leftMax - height[left];
            }
            left++;
        } else {
            if (height[right] >= rightMax) {
                rightMax = height[right];
            } else {
                waterTrapped += rightMax - height[right];
            }
            right--;
        }
    }
    return waterTrapped;
}
```

### Using a Monotonic Stack

This approach uses a stack to efficiently find boundaries where water can be trapped by identifying the current boundaries.

#### Intuition:
Use a stack to store indices of the bars. Traverse the bars and when a bar taller than the one at the stack’s top is found, calculate the trapped water by popping from the stack and using the difference between the current and popped indices.

#### Time Complexity:
- O(n), where n is the number of elements in the array.

#### Space Complexity:
- O(n), due to the use of a stack.

```javascript
function trap(height) {
    let stack = [];
    let waterTrapped = 0;
    let current = 0;

    while (current < height.length) {
        while (stack.length !== 0 && height[current] > height[stack[stack.length - 1]]) {
            let top = stack.pop();
            if (stack.length === 0)
                break;

            let distance = current - stack[stack.length - 1] - 1;
            let boundedHeight = Math.min(height[current], height[stack[stack.length - 1]]) - height[top];
            waterTrapped += distance * boundedHeight;
        }
        stack.push(current++);
    }
    return waterTrapped;
}
```

Each of the solutions gradually optimizes upon the previous approach with an improvement on the time or space complexity while ensuring correctness. Choose an approach based on the constraints and requirements of your specific use case!

