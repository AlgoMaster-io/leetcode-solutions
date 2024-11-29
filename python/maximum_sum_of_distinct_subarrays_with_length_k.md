# [2461. Maximum Sum of Distinct Subarrays With Length K](https://leetcode.com/problems/maximum-sum-of-distinct-subarrays-with-length-k/)

## Approach 1: Brute Force (Basic)

### Solution
python
```python
# Time Complexity: O(n * k)
# Space Complexity: O(k)
def maximumSubarraySum(nums, k):
    max_sum = 0

    # Iterate over every subarray of size k
    for i in range(len(nums) - k + 1):
        unique_elements = set()
        sum_ = 0
        is_valid = True

        for j in range(i, i + k):
            if nums[j] in unique_elements:
                is_valid = False  # Subarray contains duplicate
                break
            unique_elements.add(nums[j])
            sum_ += nums[j]

        if is_valid:
            max_sum = max(max_sum, sum_)

    return max_sum
```

## Approach 2: Sliding Window with HashMap (Optimal)

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(k)
def maximumSubarraySum(nums, k):
    max_sum = 0
    current_sum = 0
    frequency_map = {}
    left = 0

    # Sliding window
    for right in range(len(nums)):
        current_sum += nums[right]
        frequency_map[nums[right]] = frequency_map.get(nums[right], 0) + 1

        # If the window size exceeds k, shrink it
        if right - left + 1 > k:
            current_sum -= nums[left]
            frequency_map[nums[left]] -= 1
            if frequency_map[nums[left]] == 0:
                del frequency_map[nums[left]]
            left += 1

        # Check if the current window is valid (distinct elements)
        if right - left + 1 == k and len(frequency_map) == k:
            max_sum = max(max_sum, current_sum)

    return max_sum
```

