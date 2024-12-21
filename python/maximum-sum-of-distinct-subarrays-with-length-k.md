# [2461. Maximum Sum of Distinct Subarrays With Length K](https://leetcode.com/problems/maximum-sum-of-distinct-subarrays-with-length-k/)

In this problem, we need to find the maximum possible sum of a subarray of a given length `k` where all elements are distinct.

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Sliding Window + Set Approach](#sliding-window--set-approach)

---

### Brute Force Approach

This is the most intuitive approach where for each possible subarray of length `k`, we check if the elements are distinct and calculate the sum. It's not the most efficient method but is helpful for understanding the problem.

**Intuition:**
1. Iterate through all subarrays of length `k`.
2. For each subarray, check if all elements are distinct.
3. Calculate the sum if they are distinct and update the maximum sum.

**Code:**

```python
def maxSumOfDistinctSubarray(nums, k):
    n = len(nums)
    max_sum = 0
    
    for i in range(n - k + 1):
        current_window = nums[i:i+k]
        
        # Check if all elements in the window are distinct
        if len(set(current_window)) == k:  # Convert to set and check size
            current_sum = sum(current_window)
            max_sum = max(max_sum, current_sum)
    
    return max_sum
```

**Time Complexity:** O(n*k) - for each subarray of length `k`, we calculate the sum and check distinctness.

**Space Complexity:** O(k) - for storing the distinct elements in the set for each subarray.

---

### Sliding Window + Set Approach

To optimize, we use a sliding window with a set to maintain the distinctness of the subarray. The sliding window helps us in managing the subarray size and the set helps in maintaining distinct elements.

**Intuition:**
1. Use a sliding window of size `k` and a set to ensure all elements are distinct.
2. Start with two pointers, both at the beginning of the array.
3. Expand the window by adding elements to it.
4. Use a set to track elements present in the current window.
5. If a duplicate is found, shrink the window until the duplicate is removed.
6. Calculate the sum for valid windows and keep track of the maximum sum.

**Code:**

```python
def maxSumOfDistinctSubarray(nums, k):
    n = len(nums)
    max_sum = 0
    current_sum = 0
    current_set = set()
    left = 0

    for right in range(n):
        # Check and remove duplicates
        while nums[right] in current_set:
            current_set.remove(nums[left])
            current_sum -= nums[left]
            left += 1

        # Add the current element to the window
        current_set.add(nums[right])
        current_sum += nums[right]

        # Check if the window is of size k
        if right - left + 1 == k:
            max_sum = max(max_sum, current_sum)

            # Slide the window forward by removing the left element
            current_set.remove(nums[left])
            current_sum -= nums[left]
            left += 1
    
    return max_sum
```

**Time Complexity:** O(n) - each element is added and removed from the window at most once.

**Space Complexity:** O(k) - for storing the distinct elements of a window in a set.

This approach efficiently handles the requirement for a dynamic window size while maintaining all elements unique, giving an optimal solution for the problem.

