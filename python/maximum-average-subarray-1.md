# Maximum Average Subarray I 
You are given an integer array nums consisting of n elements, and an integer k. Find the contiguous subarray of length k that has the maximum average value and return this value.

### Constraints:
- n == nums.length
- 1 <= k <= n <= 10^5
- -10^4 <= nums[i] <= 10^4

### Examples
```javascript
Input: nums = [1,12,-5,-6,50,3], k = 4
Output: 12.75
Explanation: Maximum average is (12 - 5 - 6 + 50) / 4 = 12.75

Input: nums = [5], k = 1
Output: 5.0
```

## Approaches to Solve the Problem
### Approach 1: Brute Force (Inefficient)
##### Intuition:
A brute force approach would involve checking all possible subarrays of length k and calculating their averages. This can be done by looping over each possible starting index, calculating the sum of the subarray, and keeping track of the maximum sum.

Steps:
1. Loop through the array from index 0 to n - k.
2. For each index i, calculate the sum of the subarray starting at i and ending at i + k - 1.
3. Track the maximum sum.
4. Return the maximum sum divided by k as the maximum average.
##### Time Complexity:
O(n * k), where n is the length of the array. For each element, we compute the sum of a subarray of length k.
##### Space Complexity:
O(1), as no extra space is used beyond the variables.
##### Python Code:
```python
def findMaxAverage(nums: List[int], k: int) -> float:
    max_sum = float('-inf')
    n = len(nums)
    
    for i in range(n - k + 1):
        current_sum = sum(nums[i:i+k])
        max_sum = max(max_sum, current_sum)
    
    return max_sum / k
```
### Approach 2: Sliding Window (Optimal Solution)
##### Intuition: 
The brute force approach is inefficient because we are recalculating the sum of each subarray from scratch. Instead, we can use the sliding window technique to optimize this. The sliding window technique allows us to update the sum of the subarray by subtracting the element that is sliding out of the window and adding the element that is sliding into the window.

Steps:
1. Calculate the sum of the first k elements.
2. For each subsequent subarray, update the sum by subtracting the element that is leaving the window (at index i - k) and adding the element that is entering the window (at index i).
3. Track the maximum sum.
4. Return the maximum sum divided by k as the maximum average.
##### Visualization:
```perl
For nums = [1,12,-5,-6,50,3] and k = 4:

Initial window: [1, 12, -5, -6], sum = 2
Move window:
New window: [12, -5, -6, 50], sum = 51 (max updated)
Move window:
New window: [-5, -6, 50, 3], sum = 42 (max stays 51)
```
##### Time Complexity:
O(n), where n is the length of the array. We calculate the sum for each subarray by adjusting the previous sum in constant time.
##### Space Complexity:
O(1), as no extra space is used beyond the variables for the sliding window.
##### Python Code:
```python
def findMaxAverage(nums: List[int], k: int) -> float:
    # Step 1: Calculate the sum of the first k elements
    current_sum = sum(nums[:k])
    max_sum = current_sum
    
    # Step 2: Slide the window over the array
    for i in range(k, len(nums)):
        current_sum += nums[i] - nums[i - k]  # Slide the window
        max_sum = max(max_sum, current_sum)    # Update max sum
    
    # Step 3: Return the maximum average
    return max_sum / k
```
#### Edge Cases:
1. Single element array: If k == 1 and the array has only one element, the result is just that element.
2. All elements are the same: The sliding window will return the average of any subarray, as all values are the same.
3. Negative numbers: The solution should correctly handle arrays with negative numbers and return the correct maximum average.
## Summary
| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Brute Force	                    | O(n * k)      | O(1)             |
| Sliding Window (Optimal)	                          | O(n)            | O(1)             |

The Sliding Window approach is the most efficient solution, allowing us to find the maximum average subarray in O(n) time. It leverages the fact that we can update the sum incrementally rather than recalculating it from scratch.