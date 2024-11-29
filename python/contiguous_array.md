# 525. [Contiguous Array](https://leetcode.com/problems/contiguous-array/)

## Approach 1: Brute Force (Basic)

### Solution
python
```python
# Time Complexity: O(n^2)
# Space Complexity: O(1)
class Solution:
    def findMaxLength(self, nums):
        maxLength = 0

        # Check all possible subarrays
        for i in range(len(nums)):
            count = 0

            for j in range(i, len(nums)):
                # Increment or decrement count based on value
                count += 1 if nums[j] == 1 else -1

                # If count is zero, it means equal number of 0s and 1s
                if count == 0:
                    maxLength = max(maxLength, j - i + 1)

        return maxLength
```

## Approach 2: HashMap (Optimal)

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
class Solution:
    def findMaxLength(self, nums):
        count_map = {0: -1}  # Initialize with a count of 0 at index -1
        maxLength = 0
        count = 0

        for i in range(len(nums)):
            # Increment or decrement count based on value
            count += 1 if nums[i] == 1 else -1

            if count in count_map:
                # Calculate the length of the subarray
                maxLength = max(maxLength, i - count_map[count])
            else:
                # Store the first occurrence of this count
                count_map[count] = i

        return maxLength
```

