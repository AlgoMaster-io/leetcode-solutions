# [Leetcode 53: Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

## Approaches
1. [Bruteforce Approach](#approach-1-bruteforce-approach)
2. [Dynamic Programming (Kadane's Algorithm)](#approach-2-dynamic-programming-kadanes-algorithm)

### Approach 1: Bruteforce Approach

**Intuition**:  
The brute force approach is straightforward. We simply evaluate the sum of all possible subarrays and keep track of the maximum sum encountered during the evaluations.

- The outer loop will determine the starting point of the subarray.
- The inner loop will determine the ending point of the subarray, thereby iterating through every possibility of the subarray sequence.
- Each inner loop completion calculates the sum of the current subarray and updates the maximum sum if it is greater than the current maximum.

```python
def maxSubArray(nums):
    max_sum = float('-inf')  # Initialize to negative infinity to handle all negative arrays

    # Iterate over each possible starting point of the subarray
    for start in range(len(nums)):
        current_sum = 0
        # Iterate over each possible ending point of the subarray
        for end in range(start, len(nums)):
            current_sum += nums[end]  # Sum all elements in the current subarray
            max_sum = max(max_sum, current_sum)  # Update max_sum if current_sum is larger

    return max_sum
```

- **Time Complexity**: O(n^2), where n is the number of elements in the input array. We iterate through all possible subarrays.
- **Space Complexity**: O(1), as we only use extra space for a few variable storage.

### Approach 2: Dynamic Programming (Kadane's Algorithm)

**Intuition**:  
Kadane's Algorithm efficiently solves this problem by observing that the maximum subarray ending at a particular index can be derived from the maximum subarray ending at the previous index. This allows calculating the sum in a linear pass through the array.

- Initialize a variable to keep track of the maximum sum encountered so far (`max_sum`) and another variable to maintain the current subarray sum (`current_sum`).
- Traverse the array while updating `current_sum` by adding the current element. If `current_sum` becomes less than the current element, reset `current_sum` to the current element. This step handles negative running sums effectively.
- Update `max_sum` with the maximum of itself and `current_sum`.

```python
def maxSubArray(nums):
    max_sum = nums[0]  # Start with the first element as the initial max
    current_sum = nums[0]  # Start with the first element in the running sum

    # Start iterating from the second element
    for i in range(1, len(nums)):
        # If current_sum is improved by starting a new subarray at i, reset current_sum
        current_sum = max(nums[i], current_sum + nums[i])
        # Update max_sum to be the maximum encountered sum so far
        max_sum = max(max_sum, current_sum)

    return max_sum
```

- **Time Complexity**: O(n), where n is the number of elements in the input array. We iterate through the array once.
- **Space Complexity**: O(1), because we only use a constant amount of extra space for the variables.

Kadane's Algorithm is preferred for its linear time performance and simplicity, making it ideal for large input sizes.

