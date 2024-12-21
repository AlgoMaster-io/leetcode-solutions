# [Leetcode 209: Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

## Approaches
1. [Brute-force Approach](#brute-force-approach)
2. [Sliding Window Approach](#sliding-window-approach)

### Brute-force Approach

#### Intuition
The brute-force solution involves checking every possible subarray to determine if its sum is at least the target. If it is, we check its length and update the minimum length encountered so far. This approach is straightforward but inefficient, as it involves nested loops.

#### Steps
1. Initialize `min_length` with infinity to track the smallest length of the subarray with a sum at least equal to the target.
2. Use a nested loop where the outer loop marks the start and the inner loop finds the end of the subarray.
3. For each starting point, iterate through possible end points, calculating the sum.
4. If the sum reaches or exceeds the target, compare and possibly update `min_length`.
5. Return `0` if no subarray is found; otherwise, return `min_length`.

```python
def minSubArrayLen(target, nums):
    n = len(nums)
    min_length = float('inf')  # Start with a large number to find the minimum

    for start in range(n):
        sum = 0
        for end in range(start, n):
            sum += nums[end]
            if sum >= target:
                min_length = min(min_length, end - start + 1)
                break  # Once the target is met, no need to extend this subarray
    
    return 0 if min_length == float('inf') else min_length
```

#### Complexity
- **Time Complexity**: O(n^2), where n is the length of the array. This is due to the double loop over the array elements.
- **Space Complexity**: O(1), as we use only a constant amount of space beyond the input array.

### Sliding Window Approach

#### Intuition
The sliding window approach efficiently finds the minimum length subarray by maintaining a dynamic window and expanding or contracting it based on the current sum. This method improves upon the brute-force by reducing unnecessary recalculations and focusing on optimal window resizing.

#### Steps
1. Initialize two pointers `left` and `right` at the start of the array, and `current_sum` to track the sum of the current window.
2. Move `right` forward to include elements in the sum until it meets or exceeds the target.
3. Once the target is met, try to shrink the window from the left to potentially find a smaller valid subarray, updating the minimum length as needed.
4. Return `0` if no subarray is found; otherwise, return the length of the smallest subarray.

```python
def minSubArrayLen(target, nums):
    n = len(nums)
    min_length = float('inf')  # Initially set to a large number
    left = 0
    current_sum = 0

    for right in range(n):
        current_sum += nums[right]

        # Try to shrink the window from the left as far as the sum allows
        while current_sum >= target:
            min_length = min(min_length, right - left + 1)
            current_sum -= nums[left]
            left += 1  # Move the left boundary to try and find smaller subarray
    
    return 0 if min_length == float('inf') else min_length
```

#### Complexity
- **Time Complexity**: O(n), where n is the length of the array, as each element is processed at most twice by the pointers.
- **Space Complexity**: O(1), since we are using only a fixed amount of additional space.

By using the sliding window technique, we significantly improve the efficiency of solving the problem without sacrificing accuracy.

