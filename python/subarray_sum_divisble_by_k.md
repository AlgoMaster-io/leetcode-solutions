# [974. Subarray Sums Divisible by K](https://leetcode.com/problems/subarray-sums-divisible-by-k/)

## Approach 1: Brute Force (Basic)

### Solution
python
```python
# Time Complexity: O(n^2)
# Space Complexity: O(1)
class Solution:
    def subarraysDivByK(self, nums, k):
        count = 0

        # Check all subarrays
        for i in range(len(nums)):
            sum = 0

            for j in range(i, len(nums)):
                sum += nums[j]

                # Check if the sum is divisible by k
                if sum % k == 0:
                    count += 1

        return count
```

## Approach 2: Prefix Sum with HashMap (Optimal)

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(k)
class Solution:
    def subarraysDivByK(self, nums, k):
        remainder_map = {0: 1}  # Initialize with a remainder of 0

        count = 0
        sum = 0

        for num in nums:
            sum += num
            remainder = sum % k

            # Normalize the remainder to always be non-negative
            if remainder < 0:
                remainder += k

            # If the remainder exists in the map, add its frequency to the count
            if remainder in remainder_map:
                count += remainder_map[remainder]

            # Update the frequency of the current remainder
            remainder_map[remainder] = remainder_map.get(remainder, 0) + 1

        return count
```

