## [Leetcode 11: Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

### Navigation
- [Brute Force Approach](#brute-force-approach)
- [Two-pointer Approach](#two-pointer-approach)

## Brute Force Approach

### Intuition
The brute force approach involves checking all possible pairs of lines and calculating the amount of water that can be contained between each pair. This involves iterating through each line, pairing it with every other line, and calculating the container area. The area is determined by taking the minimum of the two heights (because the water can only be as high as the shortest line) and multiplying it by the distance between the two lines (which is simply the difference in indices).

### Approach
1. Initialize a variable `maxArea` to store the maximum area found.
2. Iterate over each line using two nested loops.
   - Outer loop `i` will select the first line.
   - Inner loop `j` will select the second line.
3. Calculate the area between lines at index `i` and `j`.
   - Height of the container is `min(height[i], height[j])`.
   - Width of the container is `(j - i)`.
   - Area is `height * width`.
4. If the calculated area is greater than `maxArea`, update `maxArea`.
5. Return `maxArea` as the result.

### Code
```csharp
public class Solution {
    public int MaxArea(int[] height) {
        int maxArea = 0; // Initialize maximum area to 0
        int n = height.Length;

        // Iterate over each pair of lines
        for (int i = 0; i < n - 1; i++) {
            for (int j = i + 1; j < n; j++) {
                // Calculate area
                int area = Math.Min(height[i], height[j]) * (j - i);
                // Update maxArea if needed
                if (area > maxArea) {
                    maxArea = area;
                }
            }
        }
        return maxArea;
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(n^2) - We iterate over all possible pairs of lines.
- **Space Complexity**: O(1) - We only use a constant amount of extra space.

## Two-pointer Approach

### Intuition
The two-pointer approach optimizes the brute force solution by using two pointers, starting from both ends of the array and working towards the center. The idea is to maximize the width while adjusting the height strategically. By incrementally adjusting the shorter line (the limiting factor for water containment), we can potentially find a taller line closer towards the center to increase the area since width naturally decreases as pointers move closer.

### Approach
1. Initialize two pointers: `left` at the beginning and `right` at the end of the array.
2. Initialize `maxArea` to store the maximum area found.
3. While `left` is less than `right`:
   - Calculate current area using `min(height[left], height[right]) * (right - left)`.
   - Update `maxArea` if the current area is larger.
   - Move the pointer from the shorter line:
     - If `height[left] < height[right]`, increment the `left` pointer.
     - Else, decrement the `right` pointer.
4. Return `maxArea`.

### Code
```csharp
public class Solution {
    public int MaxArea(int[] height) {
        int maxArea = 0; // Initialize maximum area to 0
        int left = 0; // Start pointer at the beginning
        int right = height.Length - 1; // Start pointer at the end

        // Continue until the two pointers meet
        while (left < right) {
            // Calculate the area with the current pair of lines
            int area = Math.Min(height[left], height[right]) * (right - left);
            // Update maxArea if needed
            maxArea = Math.Max(maxArea, area);

            // Move the pointer corresponding to the shorter line
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

### Complexity Analysis
- **Time Complexity**: O(n) - Each element is visited at most once, as the left and right pointers converge.
- **Space Complexity**: O(1) - We only use a constant amount of extra space.

