# 11. [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

## Approach 1: Brute Force

### Solution
typescript
```typescript
// Time Complexity: O(n^2)
// Space Complexity: O(1)
class Solution {
    maxArea(height: number[]): number {
        let maxArea = 0;

        // Check all pairs of lines
        for (let i = 0; i < height.length; i++) {
            for (let j = i + 1; j < height.length; j++) {
                // Calculate the area for the current pair of lines
                const area = Math.min(height[i], height[j]) * (j - i);
                maxArea = Math.max(maxArea, area); // Update max area if needed
            }
        }

        return maxArea;
    }
}
```

## Approach 2: Two Pointers (Optimal)

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
    maxArea(height: number[]): number {
        let left = 0; // Start pointer
        let right = height.length - 1; // End pointer
        let maxArea = 0; // Initialize max area

        // Iterate until the two pointers meet
        while (left < right) {
            // Calculate the current area
            const area = Math.min(height[left], height[right]) * (right - left);
            maxArea = Math.max(maxArea, area); // Update max area if needed

            // Move the pointer pointing to the shorter line
            if (height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }

        return maxArea;
    }
}
```

