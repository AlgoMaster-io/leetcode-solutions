# [42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

## Approach 1: Brute Force (Basic)

### Solution
python
```python
# Time Complexity: O(n^2)
# Space Complexity: O(1)
class Solution:
    def trap(self, height):
        n = len(height)
        totalWater = 0

        # Iterate over each bar
        for i in range(1, n - 1):
            leftMax = 0
            rightMax = 0

            # Find the maximum height to the left of the current bar
            for j in range(0, i + 1):
                leftMax = max(leftMax, height[j])

            # Find the maximum height to the right of the current bar
            for j in range(i, n):
                rightMax = max(rightMax, height[j])

            # Calculate water trapped at the current bar
            totalWater += min(leftMax, rightMax) - height[i]

        return totalWater
```

## Approach 2: Dynamic Programming

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
class Solution:
    def trap(self, height):
        n = len(height)
        if n == 0:
            return 0

        leftMax = [0] * n
        rightMax = [0] * n
        totalWater = 0

        # Compute leftMax array
        leftMax[0] = height[0]
        for i in range(1, n):
            leftMax[i] = max(leftMax[i - 1], height[i])

        # Compute rightMax array
        rightMax[n - 1] = height[n - 1]
        for i in range(n - 2, -1, -1):
            rightMax[i] = max(rightMax[i + 1], height[i])

        # Calculate water trapped at each bar
        for i in range(n):
            totalWater += min(leftMax[i], rightMax[i]) - height[i]

        return totalWater
```

## Approach 3: Two Pointers (Optimal)

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def trap(self, height):
        left, right = 0, len(height) - 1
        leftMax, rightMax = 0, 0
        totalWater = 0

        # Use two pointers to calculate trapped water
        while left < right:
            if height[left] < height[right]:
                # Update leftMax and calculate water at left pointer
                if height[left] >= leftMax:
                    leftMax = height[left]
                else:
                    totalWater += leftMax - height[left]
                left += 1
            else:
                # Update rightMax and calculate water at right pointer
                if height[right] >= rightMax:
                    rightMax = height[right]
                else:
                    totalWater += rightMax - height[right]
                right -= 1

        return totalWater
```

