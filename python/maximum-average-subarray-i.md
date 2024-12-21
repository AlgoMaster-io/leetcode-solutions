# [Leetcode 643: Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/)

## Approaches:
1. [Brute Force Approach](#1-brute-force-approach)
2. [Sliding Window Approach](#2-sliding-window-approach)

### 1. Brute Force Approach

#### Intuition:
The brute force solution involves calculating the sum of every subarray of length `k` and then dividing it by `k` to find the average. This method is straightforward but not optimal, as it requires iterating over the array to calculate the sum for each subarray.

#### Code:
```python
def findMaxAverage(nums, k):
    n = len(nums)
    max_avg = float('-inf')  # Initialize with the smallest possible value
    
    # Iterate over each starting index of subarray of length k
    for i in range(n - k + 1): 
        current_sum = 0
        # Calculate the sum for the current subarray of length k
        for j in range(i, i + k):
            current_sum += nums[j]
        # Calculate current average and compare with max_avg
        current_avg = current_sum / k
        if current_avg > max_avg:
            max_avg = current_avg
    
    return max_avg
```

#### Time Complexity:
- The time complexity is O(n*k), where `n` is the length of the array. 
- This complexity arises because for each starting index `i`, we perform an additional iteration of length `k`.

#### Space Complexity:
- The space complexity is O(1) since we use only a fixed amount of extra space.

### 2. Sliding Window Approach

#### Intuition:
The sliding window technique helps optimize the problem by maintaining a window of size `k` that slides through the array. By adding the next element in the array and removing the first element of the current window, you compute the sum for the next window in constant time. This reduces repeated computations of sums from overlapping subarrays, thereby enhancing efficiency.

#### Code:
```python
def findMaxAverage(nums, k):
    n = len(nums)
    # Calculate the initial sum of the first subarray of length k
    current_sum = sum(nums[:k])
    max_sum = current_sum  # Initialize max_sum with current_sum

    # Slide the window over the array from the (k+1)th element to the end
    for i in range(k, n):
        # Update the sum by adding the new element and removing the oldest element
        current_sum += nums[i] - nums[i - k]
        # Update max_sum if the new current_sum is greater
        if current_sum > max_sum:
            max_sum = current_sum
    
    # Return the maximum average
    return max_sum / k
```

#### Time Complexity:
- The time complexity is O(n), where `n` is the length of the array.
- This is because each element is processed at most twice (once it enters the window, and once it exits).

#### Space Complexity:
- The space complexity is O(1) as we only use a constant amount of extra space for the sum and loop variables.

