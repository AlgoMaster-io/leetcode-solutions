# Container With Most Water
You are given an integer array height of length n. There are n vertical lines drawn such that the two endpoints of the i-th line are (i, 0) and (i, height[i]).

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return the maximum amount of water a container can store.

### Constraints:
- n == height.length
- 2 <= n <= 10^5
- 0 <= height[i] <= 10^4

### Examples
```javascript
Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
Explanation: The vertical lines are drawn at indices 0, 1, 2, ..., 8.
The two lines that form the container are at indices 1 and 8. The area of the container is (8 - 1) * min(8, 7) = 49.

Input: height = [1,1]
Output: 1
```

## Approaches to Solve the Problem
### Approach 1: Brute Force (Inefficient)
##### Intuition:
A brute-force solution involves checking the area formed by every pair of lines. The area between two lines at positions i and j is given by:
```Area = (j - i) * min(height[i], height[j])```

Steps:
1. Use two nested loops to consider every possible pair of lines.
2. For each pair of lines, calculate the area of water the container can hold.
3. Track the maximum area found.
##### Time Complexity:
O(n²), where n is the length of the array. This approach checks all possible pairs of lines.
##### Space Complexity:
O(1), no extra space is used beyond a few variables.
##### Python Code:
```python
def maxArea(height: List[int]) -> int:
    max_area = 0
    n = len(height)
    
    for i in range(n):
        for j in range(i + 1, n):
            area = (j - i) * min(height[i], height[j])
            max_area = max(max_area, area)
    
    return max_area
```
### Approach 2: Two Pointers (Optimal Solution)
##### Intuition: 
To improve efficiency, we can use the two-pointer technique. The idea is to place one pointer at the beginning (left) and another at the end (right) of the array. Then, we calculate the area and move the pointer that has the smaller height because the width is already maximized, and we want to find a taller line that may increase the area.

1. The area between two lines is determined by the shorter of the two lines and the distance between them.
2. By moving the shorter line inward, we have a chance of finding a taller line that might increase the area.

Steps:
1. Initialize two pointers, left at the beginning and right at the end of the array.
2. While left is less than right, calculate the area between the two lines.
3. Move the pointer pointing to the shorter line inward, because only moving the shorter line could potentially increase the area.
4. Track the maximum area found.
##### Visualization:
```perl
For height = [1,8,6,2,5,4,8,3,7]:

Initial pointers:
left = 0 (height[0] = 1)
right = 8 (height[8] = 7)

Iteration 1:
Area = (8 - 0) * min(1, 7) = 8
Move the left pointer to the right since height[0] < height[8].

Iteration 2:
left = 1 (height[1] = 8), right = 8 (height[8] = 7)
Area = (8 - 1) * min(8, 7) = 49
Move the right pointer to the left since height[8] < height[1].

Continue until the pointers meet.
...

Continue until all triplets are checked.
```
##### Time Complexity:
O(n), where n is the length of the array. We traverse the array once.
##### Space Complexity:
O(1), as we use only two pointers and a few variables.
##### Python Code:
```python
def maxArea(height: List[int]) -> int:
    left, right = 0, len(height) - 1
    max_area = 0
    
    while left < right:
        # Calculate the area between the lines at left and right
        width = right - left
        current_height = min(height[left], height[right])
        area = width * current_height
        max_area = max(max_area, area)
        
        # Move the pointer pointing to the shorter line
        if height[left] < height[right]:
            left += 1
        else:
            right -= 1
    
    return max_area
```
##### Visualization:
```perl
For height = [1,8,6,2,5,4,8,3,7]:

Step 1:
left = 0, right = 8
Calculate Area = (8 - 0) * min(1, 7) = 8

Step 2:
Move the left pointer to the right, left = 1
Calculate Area = (8 - 1) * min(8, 7) = 49 (New Max)

Step 3:
Move the right pointer to the left, right = 7
Continue calculating and moving pointers until left = right.

Final max area: 49
```
##### Edge Cases
1. Two elements: If the array has only two elements, the solution should handle this case directly, as the only area possible is the product of the two heights and the distance between them.
2. All elements are the same height: If all elements in the array have the same height, the algorithm should return the area as the product of the first and last element's height and their distance.
3. Array with zero heights: The array may contain zeros, and the solution should correctly handle these cases as no water can be trapped between two zero heights.

| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Brute Force	                    | O(n²)      | O(1)             |
| Two Pointers	                          | O(n)            | O(1)             |

The Two Pointers approach is the most optimal solution as it efficiently reduces the problem to a linear scan, avoiding the need for checking every pair of lines, and uses constant space.