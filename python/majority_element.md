# [169. Majority Element](https://leetcode.com/problems/majority-element/)

## Approach 1: Brute Force

### Solution
```python
# Time Complexity: O(n^2)
# Space Complexity: O(1)
class Solution:
    def majorityElement(self, nums):
        majority_count = len(nums) // 2

        # Check the count of each element
        for num in nums:
            count = 0
            for element in nums:
                if element == num:
                    count += 1
            # If count exceeds majority, return the element
            if count > majority_count:
                return num

        return -1 # Shouldn't reach here as per problem constraints
```

## Approach 2: HashMap to Count Frequencies

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
from collections import defaultdict

class Solution:
    def majorityElement(self, nums):
        count_map = defaultdict(int)
        majority_count = len(nums) // 2

        # Count frequencies of elements
        for num in nums:
            count_map[num] += 1

            # If an element's count exceeds majority, return it
            if count_map[num] > majority_count:
                return num

        return -1 # Shouldn't reach here as per problem constraints
```

## Approach 3: Sorting

### Solution
```python
# Time Complexity: O(n log n)
# Space Complexity: O(1)
class Solution:
    def majorityElement(self, nums):
        # Sort the array
        nums.sort()

        # The majority element will always be at the middle index
        return nums[len(nums) // 2]
```

## Approach 4: Boyer-Moore Voting Algorithm

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def majorityElement(self, nums):
        count = 0
        candidate = None

        # Find the candidate for the majority element
        for num in nums:
            if count == 0:
                candidate = num # Set a new candidate
            count += (1 if num == candidate else -1)

        return candidate # The problem guarantees the majority element exists
```

