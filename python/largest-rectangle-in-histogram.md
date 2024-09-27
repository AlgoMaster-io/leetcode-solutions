# Largest Rectangle in Histogram
Given an array of integers heights representing the histogram's bar heights where the width of each bar is 1, return the area of the largest rectangle in the histogram.

### Constraints:
- 1 <= heights.length <= 10^5
- 0 <= heights[i] <= 10^4

### Examples
```javascript
Input: heights = [2,1,5,6,2,3]
Output: 10
Explanation: The largest rectangle is formed by the bars of heights [5,6] with width 2, giving area 5 * 2 = 10.

Input: heights = [2,4]
Output: 4
Explanation: The largest rectangle is the single bar of height 4 with width 1, giving area 4 * 1 = 4.
```

## Approaches to Solve the Problem
### Approach 1: Brute Force (Inefficient)
##### Intuition:
For every bar, we can try to expand the rectangle to the left and right until we hit a bar that is shorter. For each bar, calculate the maximum rectangle that can be formed using that bar as the shortest. This involves calculating the width that extends as far as we can to the left and right for each bar.

Steps:
1. For each bar at index i, check how far left and right you can extend while maintaining the rectangle height.
2. For every bar, compute the rectangle area, then return the maximum area found.
##### Time Complexity:
O(n²), where n is the number of bars. For each bar, we iterate left and right to find the largest rectangle.
##### Space Complexity:
O(1), because we only use a few variables to track the maximum area.
##### Python Code:
```python
def largestRectangleArea(heights):
    n = len(heights)
    max_area = 0
    
    for i in range(n):
        height = heights[i]
        left = i
        right = i
        
        # Extend to the left
        while left > 0 and heights[left - 1] >= height:
            left -= 1
        
        # Extend to the right
        while right < n - 1 and heights[right + 1] >= height:
            right += 1
        
        # Calculate area with heights[i] as the smallest bar
        max_area = max(max_area, height * (right - left + 1))
    
    return max_area
```

### Approach 2: Stack-based Monotonic Approach (Optimal Solution)
##### Intuition: 
The brute-force approach is inefficient because we repeatedly scan for the largest rectangle for each bar. Instead, we can optimize the process using a monotonic stack. The idea is to calculate the next smaller bar on both the left and right of each bar in the histogram, as this helps determine the largest possible rectangle that can be formed using each bar.

A monotonic stack can help efficiently keep track of indices of bars in the histogram. As we iterate through the bars:
- If the current bar is shorter than the bar at the top of the stack, we can calculate the area with the bar at the top as the height.
- We continue popping from the stack until we find a shorter bar or the stack becomes empty.

Steps:
1. Initialize an empty stack to store indices of bars and a variable max_area to track the largest area.
2. Traverse the array of heights.
3. For each bar:
   - While the stack is not empty and the current bar is smaller than the bar at the top of the stack:
     - Pop the top index and calculate the area of the rectangle with the popped height as the smallest bar.
     - Update max_area with the calculated area.
4. After the loop, pop remaining bars from the stack and calculate their areas.
5. Return max_area.
### Visualization
For heights = [2, 1, 5, 6, 2, 3]:

```rust
Stack process:
- Day 0: 2 → push index 0 to the stack.
- Day 1: 1 is smaller → pop index 0 (height 2), area = 2 × 1 → max_area = 2 → push index 1.
- Day 2: 5 → push index 2 to the stack.
- Day 3: 6 → push index 3 to the stack.
- Day 4: 2 is smaller → pop index 3 (height 6), area = 6 × 1 → max_area = 6 → pop index 2 (height 5), area = 5 × 2 → max_area = 10 → push index 4.
- Day 5: 3 → push index 5 to the stack.

Final pops calculate the remaining areas.
```
##### Time Complexity:
O(n), where n is the number of bars. Each bar is pushed and popped from the stack at most once.
##### Space Complexity:
O(n), for the stack that stores the indices of bars.
##### Python Code:
```python
def largestRectangleArea(heights):
    stack = []
    max_area = 0
    n = len(heights)
    
    for i in range(n):
        while stack and heights[stack[-1]] > heights[i]:
            h = heights[stack.pop()]
            width = i if not stack else i - stack[-1] - 1
            max_area = max(max_area, h * width)
        stack.append(i)
    
    # Calculate areas for remaining bars in the stack
    while stack:
        h = heights[stack.pop()]
        width = n if not stack else n - stack[-1] - 1
        max_area = max(max_area, h * width)
    
    return max_area
```
### Edge Cases:
1. Empty Array: If heights is empty, the largest rectangle area should be 0.
2. Single Element: If heights has only one bar, the largest rectangle area is the height of that bar.
3. All Bars of the Same Height: If all bars are the same height, the largest rectangle is the entire histogram with an area of height * number_of_bars.
4. Decreasing Heights: If the heights are strictly decreasing, the stack will be emptied at each new bar, resulting in the largest rectangle being the first bar.
## Summary

| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Brute Force                        | O(n²)      | O(1)             |
| Stack-based Solution (Optimal)                          | O(n)            | O(n)             |

The Stack-based Monotonic approach is the most efficient solution. It processes the bars in linear time by leveraging the stack to compute the largest rectangle that includes each bar as the shortest.