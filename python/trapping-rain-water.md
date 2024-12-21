[Leetcode Problem 42: Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

## Approaches
- [Brute Force Solution](#brute-force-solution)
- [Dynamic Programming Solution](#dynamic-programming-solution)
- [Two Pointers Solution](#two-pointers-solution)

### Brute Force Solution

The brute force solution involves iterating over each bar and calculating the amount of water that can be trapped above it. To determine the trapped water above a bar, you find the maximum height to the left and the maximum height to the right. The water above the current bar will be the minimum of these two maximum heights minus the height of the current bar.

#### Intuition
For each position in the array, the water that can be trapped is determined by the maximum boundary heights on both sides. By traversing each bar and calculating the left and right maximum heights, we determine the potential water trapped at that position.

#### Code
```python
def trap_brute_force(height):
    total_water = 0
    n = len(height)

    # Check for the minimal length of the list that violates water trapping
    for i in range(n):
        # Find the maximum height on the left up to the current point
        max_left = max(height[:i+1])
        
        # Find the maximum height on the right from the current point
        max_right = max(height[i:])
        
        # Calculate the trapped water above the current bar
        water_at_i = min(max_left, max_right) - height[i]
        
        # Add to the total water trapped
        total_water += water_at_i

    return total_water

# Example usage
# Given heights: [0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1]
# Total trapped water: 6
```

#### Time Complexity
- **Time Complexity:** O(n^2) - For each bar, we perform O(n) work to find the maximums, leading to an overall quadratic time.
- **Space Complexity:** O(1) - Only a constant amount of extra space is used.

### Dynamic Programming Solution

To enhance the efficiency, we can use two arrays to store the maximum heights to the left and right of each bar. This allows us to calculate the trapped water in O(1) time for each bar after an O(n) preprocessing step.

#### Intuition
Precomputing the maximum height to the left and right of every position decreases the repetitive work done in the brute force approach. This transformed the maximum height calculations from O(n) per element to O(1).

#### Code
```python
def trap_dynamic_programming(height):
    if not height:
        return 0

    n = len(height)
    left_max = [0] * n
    right_max = [0] * n
    total_water = 0

    # Precompute the left maximum for each bar
    left_max[0] = height[0]
    for i in range(1, n):
        left_max[i] = max(left_max[i - 1], height[i])

    # Precompute the right maximum for each bar
    right_max[n - 1] = height[n - 1]
    for i in range(n - 2, -1, -1):
        right_max[i] = max(right_max[i + 1], height[i])

    # Calculate total water trapped
    for i in range(n):
        total_water += min(left_max[i], right_max[i]) - height[i]

    return total_water

# Example usage
# Given heights: [0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1]
# Total trapped water: 6
```

#### Time Complexity
- **Time Complexity:** O(n) - Precomputing left and right maximum values takes linear time.
- **Space Complexity:** O(n) - Two arrays are used to store the left and right max for each bar.

### Two Pointers Solution

The two pointers solution uses a more optimal technique by maintaining left and right pointers and traversing the array from both ends.

#### Intuition
By using two pointers, we leverage the idea that water trapped is determined by the shorter boundary. We move the pointer of the shorter boundary inward, updating the maximum heights and water calculations dynamically.

#### Code
```python
def trap_two_pointers(height):
    left, right = 0, len(height) - 1
    left_max, right_max = 0, 0
    total_water = 0

    # Move from both ends of the array towards the center
    while left < right:
        if height[left] < height[right]:
            # If the left boundary is shorter
            if height[left] >= left_max:
                left_max = height[left]  # Update left_max if necessary
            else:
                total_water += left_max - height[left]  # Calculate water above current bar
            left += 1  # Move left pointer
        else:
            # If the right boundary is shorter
            if height[right] >= right_max:
                right_max = height[right]  # Update right_max if necessary
            else:
                total_water += right_max - height[right]  # Calculate water above current bar
            right -= 1  # Move right pointer

    return total_water

# Example usage
# Given heights: [0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1]
# Total trapped water: 6
```

#### Time Complexity
- **Time Complexity:** O(n) - Each element is visited at most once.
- **Space Complexity:** O(1) - No additional space besides a few variables is used.

