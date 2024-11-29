# [1512. Number of Good Pairs](https://leetcode.com/problems/number-of-good-pairs/)

## Approach 1: Brute Force

### Solution
python
```python
# Time Complexity: O(n^2)
# Space Complexity: O(1)
class Solution:
    def numIdenticalPairs(self, nums):
        count = 0

        # Compare every pair of elements
        for i in range(len(nums)):
            for j in range(i + 1, len(nums)):
                if nums[i] == nums[j]:
                    count += 1  # Increment count if pair is "good"

        return count
```

## Approach 2: Dictionary for Counting Frequencies

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
from collections import defaultdict

class Solution:
    def numIdenticalPairs(self, nums):
        count_map = defaultdict(int)
        count = 0

        # Count the occurrences of each number
        for num in nums:
            count += count_map[num]  # Add the current count of this number
            count_map[num] += 1  # Increment the count

        return count
```

## Approach 3: Frequency Array (Optimized for Small Range)

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(1) (assuming the range of numbers is limited)
class Solution:
    def numIdenticalPairs(self, nums):
        freq = [0] * 101  # Frequency array for numbers in range [1, 100]
        count = 0

        # Count pairs using frequency
        for num in nums:
            count += freq[num]  # Add the number of pairs that can be formed
            freq[num] += 1  # Increment the frequency of the current number

        return count
```

