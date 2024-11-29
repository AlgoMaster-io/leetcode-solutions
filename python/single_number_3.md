# 260. [Single Number III](https://leetcode.com/problems/single-number-iii/)

## Approach 1: HashMap for Counting Frequencies

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
from collections import defaultdict

class Solution:
    def singleNumber(self, nums):
        countMap = defaultdict(int)

        # Count the frequency of each number
        for num in nums:
            countMap[num] += 1

        result = []
        # Find the numbers with frequency 1
        for num, count in countMap.items():
            if count == 1:
                result.append(num)

        return result
```

## Approach 2: Bit Manipulation

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(1)

class Solution:
    def singleNumber(self, nums):
        xor = 0
        
        # XOR all numbers to get xor of the two unique numbers
        for num in nums:
            xor ^= num
        
        # Find a set bit (rightmost) in xor to separate the numbers
        diff = xor & -xor
        
        result = [0, 0]
        # Separate the numbers into two groups and XOR them respectively
        for num in nums:
            if (num & diff) == 0:
                result[0] ^= num  # XOR for first group
            else:
                result[1] ^= num  # XOR for second group
        
        return result
```

