# [11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

## Approach 1: Brute Force

### Solution
python
```python
# Time Complexity: O(n^2)
# Space Complexity: O(1)
class Solution:
    def maxArea(self, height):
        max_area = 0

        # Check all pairs of lines
        for i in range(len(height)):
            for j in range(i + 1, len(height)):
                # Calculate the area for the current pair of lines
                area = min(height[i], height[j]) * (j - i)
                max_area = max(max_area, area) # Update max area if needed

        return max_area
```

## Approach 2: Two Pointers (Optimal)

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def maxArea(self, height):
        left = 0 # Start pointer
        right = len(height) - 1 # End pointer
        max_area = 0 # Initialize max area

        # Iterate until the two pointers meet
        while left < right:
            # Calculate the current area
            area = min(height[left], height[right]) * (right - left)
            max_area = max(max_area, area) # Update max area if needed

            # Move the pointer pointing to the shorter line
            if height[left] < height[right]:
                left += 1
            else:
                right -= 1

        return max_area
```

