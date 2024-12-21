
[Leetcode Problem 11: Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

## Approaches:

1. [Brute Force](#brute-force)
2. [Two-Pointer Technique](#two-pointer-technique)

### Brute Force

The brute force approach involves considering every pair of lines and calculating the amount of water they can contain. This is achieved by using two nested loops. Key intuition is that the container is bounded by the shorter of the two lines and the distance between them.

#### Code:
```typescript
function maxAreaBruteForce(height: number[]): number {
    let maxArea = 0;
    // Iterate over all pairs of lines
    for (let i = 0; i < height.length; i++) {
        for (let j = i + 1; j < height.length; j++) {
            // Calculate the height of the container
            const currentHeight = Math.min(height[i], height[j]);
            // Calculate the width of the container
            const width = j - i;
            // Calculate the area and update maxArea if it's larger
            const currentArea = currentHeight * width;
            if (currentArea > maxArea) {
                maxArea = currentArea;
            }
        }
    }
    return maxArea;
}
```

#### Time Complexity:
- O(n^2) where n is the length of the input array. This is because we compare each possible pair of lines.
  
#### Space Complexity:
- O(1) as we are only using a constant amount of additional space beyond the input array.

### Two-Pointer Technique

The two-pointer technique provides an optimized solution by starting with two pointers, one at the beginning and one at the end of the input array. The idea is to maximize the container's width and then attempt to maximize its height by moving the pointer at the shorter line inwards.

#### Intuition:
- The further the lines are apart, the larger the width. So start with the widest possible container.
- Reduce the width if and only if there's a potential to increase the height (ie. move the shorter bar inwards).

#### Code:
```typescript
function maxAreaTwoPointer(height: number[]): number {
    let maxArea = 0;
    let left = 0;
    let right = height.length - 1;

    // Iterate until the two pointers meet
    while (left < right) {
        // Calculate the height and width of the current container
        const currentHeight = Math.min(height[left], height[right]);
        const width = right - left;
        // Calculate the current area
        const currentArea = currentHeight * width;
        // Update maxArea if the current area is bigger
        maxArea = Math.max(maxArea, currentArea);

        // Move the pointer that points to the shorter line
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
- O(n) where n is the length of the input array. We only iterate over the array once.

#### Space Complexity:
- O(1) as we use only a constant amount of additional space.

Both methods solve the problem, but the two-pointer approach is much more efficient for larger input sizes.

