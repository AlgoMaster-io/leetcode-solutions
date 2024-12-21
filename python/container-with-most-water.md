# [Leetcode 11: Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

## Approaches
1. [Brute Force Solution](#brute-force-solution)
2. [Two Pointers Approach](#two-pointers-approach)

### Brute Force Solution

#### Intuition
In the brute force approach, we can consider every pair of lines and calculate the amount of water they can contain. This can be done by iterating over all possible pairs of lines and calculating the area for each pair. Although this approach is straightforward, it is not efficient because it evaluates every possible pair which results in a quadratic time complexity.

#### Algorithm
1. Initialize `max_area` to 0.
2. Use a nested loop where the outer loop selects the first line and the inner loop selects the second line.
3. For each pair of lines, calculate the area using the formula: `min(height[i], height[j]) * (j - i)`.
4. Update `max_area` with the maximum of its current value and the calculated area.
5. Return `max_area`.

#### Code

```python
def max_area_brute_force(height):
    max_area = 0
    n = len(height)
    
    # Try all possible pairs to calculate the maximum area
    for i in range(n):
        for j in range(i + 1, n):
            # Calculate the area with height of the shorter line as the limiting height
            area = min(height[i], height[j]) * (j - i)
            max_area = max(max_area, area)
    
    return max_area
```

#### Complexity Analysis
- **Time Complexity**: \(O(n^2)\) where \(n\) is the number of lines. This is due to the nested loops over the input array.
- **Space Complexity**: \(O(1)\) because we are using only a constant amount of space to store variables.

### Two Pointers Approach

#### Intuition
The two pointers technique optimizes the solution by using the fact that the area is limited by the shorter of the two lines. By starting with two pointers at the beginning and end of the array, we can gradually move towards the center, trying to find a configuration that can provide a larger area with each step. When we move the shorter of the two lines inward, we have a chance to increase the area because we are increasing the distance between the two lines while potentially finding a taller line.

#### Algorithm
1. Initialize two pointers, `left` at 0 and `right` at the last index.
2. Initialize `max_area` to 0.
3. While `left` is less than `right`:
   - Calculate `width` as the difference between `right` and `left`.
   - Calculate `area` as `min(height[left], height[right]) * width`.
   - Update `max_area` if the calculated area is greater.
   - Move the pointer pointing to the shorter line inward:
     - If `height[left]` is less than `height[right]`, increment `left`.
     - Otherwise, decrement `right`.
4. Continue the above process till `left` is not less than `right`.
5. Return `max_area`.

#### Code

```python
def max_area_two_pointers(height):
    left, right = 0, len(height) - 1
    max_area = 0
    
    # Use two pointers narrowing approach
    while left < right:
        # Calculate width and area
        width = right - left
        area = min(height[left], height[right]) * width
        # Update max_area
        max_area = max(max_area, area)
        
        # Move the pointer of the shorter line
        if height[left] < height[right]:
            left += 1
        else:
            right -= 1
        
    return max_area
```

#### Complexity Analysis
- **Time Complexity**: \(O(n)\) because we only need to scan the array once with two pointers.
- **Space Complexity**: \(O(1)\) since we do not use any additional data structures and only a constant amount of space is required for variables.

