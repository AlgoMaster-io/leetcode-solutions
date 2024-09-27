# Find Peak Element
A peak element is an element that is strictly greater than its neighbors.

Given an integer array nums, find a peak element, and return its index. If the array contains multiple peaks, return the index to any of the peaks.

You may imagine that nums[-1] = -∞ and nums[n] = -∞ (i.e., the boundary elements are considered lower than all elements).

You must write an algorithm that runs in O(log n) time.
### Constraints:
- 1 <= nums.length <= 1000
- -2^31 <= nums[i] <= 2^31 - 1
- nums[i] != nums[i + 1] for all valid i.

### Examples
```javascript
Input: nums = [1,2,3,1]
Output: 2
Explanation: 3 is a peak element and its index is 2.

Input: nums = [1,2,1,3,5,6,4]
Output: 5
Explanation: Your function can return index 5 (the element 6 is a peak) or index 1 (the element 2 is a peak).
```

## Approaches to Solve the Problem
### Approach 1: Linear Search (Brute Force)
##### Intuition:
A simple approach is to traverse the array linearly and check for each element if it is greater than both of its neighbors. If such an element is found, return its index.

Steps:
1. Iterate through the array from 0 to n - 1.
2. For each element nums[i], check if it is greater than both nums[i-1] and nums[i+1] (handling boundary cases where i == 0 or i == n - 1).
3. If an element satisfies this condition, return the index i.
##### Time Complexity:
O(n), where n is the length of the array. In the worst case, we may need to scan the entire array.
##### Space Complexity:
O(1), because we only use a few variables to track indices and comparisons.
##### Python Code:
```python
def findPeakElement(nums):
    for i in range(len(nums)):
        if (i == 0 or nums[i] > nums[i - 1]) and (i == len(nums) - 1 or nums[i] > nums[i + 1]):
            return i
```
### Approach 2: Binary Search (Optimal Solution)
##### Intuition: 
We can use binary search to find a peak element efficiently in O(log n) time. The key observation is that if nums[mid] is greater than nums[mid + 1], then the peak must lie on the left side (including mid), and if nums[mid] is less than nums[mid + 1], then the peak must lie on the right side (excluding mid).

The reason behind this is that if the middle element is smaller than its neighbor, there must be a peak on the side with the larger neighbor, because eventually the array must "fall" to the boundaries where we consider the elements as -∞.

Steps:
1. Initialize two pointers left = 0 and right = len(nums) - 1.
2. While left < right:
   - Calculate the midpoint: mid = (left + right) // 2.
   - If nums[mid] > nums[mid + 1], it means the peak is in the left half (including mid), so move the right pointer to mid.
   - If nums[mid] < nums[mid + 1], it means the peak is in the right half (excluding mid), so move the left pointer to mid + 1.
3. Once left == right, return left (or right), which points to a peak element.
##### Visualization:
For nums = [1, 2, 1, 3, 5, 6, 4]:

```perl
Initial: left = 0, right = 6
Iteration 1: mid = 3 → nums[mid] = 3, nums[mid + 1] = 5 → move left to mid + 1
Iteration 2: mid = 5 → nums[mid] = 6, nums[mid + 1] = 4 → move right to mid
Final result: left = 5, right = 5 → peak is at index 5 with value 6
```
##### Time Complexity:
O(log n), where n is the length of the array. We reduce the search space by half with each iteration.
##### Space Complexity:
O(1), because we only use a few variables for the binary search process.
##### Python Code:
```python
def findPeakElement(nums):
    left, right = 0, len(nums) - 1
    
    while left < right:
        mid = (left + right) // 2
        if nums[mid] > nums[mid + 1]:
            right = mid
        else:
            left = mid + 1
    
    return left  # or return right, both will point to the peak
```
### Edge Cases:
1. Single Element: If the array has only one element, that element is trivially the peak.
2. Two Elements: If the array has two elements, the peak is the larger of the two.
3. All Increasing or Decreasing: If the array is strictly increasing, the last element is the peak. If the array is strictly decreasing, the first element is the peak.
## Summary

| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Brute Force                    | O(n)      | O(1)             |
| Binary Search with Pivot Handling	                          | 	O(log n)            | O(1)             |

The Binary Search approach is the most efficient solution, achieving O(log n) time complexity while using constant space. This solution takes advantage of the properties of the array and uses binary search logic to efficiently find a peak.