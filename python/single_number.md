# 136. [Single Number](https://leetcode.com/problems/single-number/)

## Approach 1: Using Dictionary

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        count_map = {}

        # Count the frequency of each number
        for num in nums:
            count_map[num] = count_map.get(num, 0) + 1

        # Find the number with frequency 1
        for num in nums:
            if count_map[num] == 1:
                return num

        return -1  # No unique number found
```

## Approach 2: Using Sorting

### Solution
python
```python
# Time Complexity: O(n log n)
# Space Complexity: O(1)
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        nums.sort()  # Sort the array

        # Traverse the array looking for unique element
        for i in range(0, len(nums) - 1, 2):
            if nums[i] != nums[i + 1]:
                return nums[i]

        return nums[-1]  # The last element is the single number
```

## Approach 3: Using XOR

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        result = 0

        # XOR all elements, duplicate elements will cancel out
        for num in nums:
            result ^= num

        return result  # The remaining single number
```

