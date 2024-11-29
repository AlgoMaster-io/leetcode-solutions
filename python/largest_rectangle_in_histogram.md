# [84. Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)

## Approach 1: Brute Force

### Solution
python
```python
# Time Complexity: O(n^2)
# Space Complexity: O(1)
class Solution:
    def largestRectangleArea(self, heights):
        max_area = 0

        # Check every pair of bars as the boundaries of the rectangle
        for i in range(len(heights)):
            min_height = heights[i]

            for j in range(i, len(heights)):
                # Update the minimum height in the current range
                min_height = min(min_height, heights[j])
                # Calculate the area and update max_area if needed
                max_area = max(max_area, min_height * (j - i + 1))

        return max_area
```

## Approach 2: Using a Stack (Optimal Solution)

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
class Solution:
    def largestRectangleArea(self, heights):
        stack = []  # Stack stores indices
        max_area = 0
        n = len(heights)

        for i in range(n + 1):
            # Use 0 height as a sentinel value for the end of the array
            current_height = 0 if i == n else heights[i]

            # Process and calculate area for indices in the stack
            while stack and current_height < heights[stack[-1]]:
                height = heights[stack.pop()]  # Height of the rectangle
                width = i if not stack else i - stack[-1] - 1  # Width of the rectangle
                max_area = max(max_area, height * width)  # Update max_area

            stack.append(i)  # Push the current index

        return max_area
```

## Approach 3: Divide and Conquer

### Solution
python
```python
# Time Complexity: O(n log n) on average, O(n^2) in the worst case
# Space Complexity: O(n) due to recursion
class Solution:
    def largestRectangleArea(self, heights):
        return self.calculate_area(heights, 0, len(heights) - 1)

    def calculate_area(self, heights, start, end):
        if start > end:
            return 0

        # Find the index of the minimum height in the range
        min_index = start
        for i in range(start, end + 1):
            if heights[i] < heights[min_index]:
                min_index = i

        # Calculate the maximum area by dividing the problem
        current_area = heights[min_index] * (end - start + 1)
        left_area = self.calculate_area(heights, start, min_index - 1)
        right_area = self.calculate_area(heights, min_index + 1, end)

        return max(current_area, max(left_area, right_area))
```

