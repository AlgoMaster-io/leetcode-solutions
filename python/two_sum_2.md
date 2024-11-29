# [167. Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

## Approach 1: Binary Search for the Complement

### Solution
python
```python
# Time Complexity: O(n log n)
# Space Complexity: O(1)
class Solution:
    def twoSum(self, numbers, target):
        def binarySearch(numbers, left, right, target):
            while left <= right:
                mid = left + (right - left) // 2
                if numbers[mid] == target:
                    return mid
                elif numbers[mid] < target:
                    left = mid + 1
                else:
                    right = mid - 1
            return -1  # Target not found

        for i in range(len(numbers)):
            complement = target - numbers[i]
            index = binarySearch(numbers, i + 1, len(numbers) - 1, complement)
            if index != -1:
                return [i + 1, index + 1]  # 1-based index
        return [-1, -1]  # No solution found
```

## Approach 2: HashMap (For Non-Sorted Input)

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
class Solution:
    def twoSum(self, numbers, target):
        num_map = {}

        for i, number in enumerate(numbers):
            complement = target - number
            if complement in num_map:
                return [num_map[complement] + 1, i + 1]  # 1-based index
            num_map[number] = i
        return [-1, -1]  # No solution found
```

## Approach 3: Two Pointers

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def twoSum(self, numbers, target):
        left = 0  # Start pointer
        right = len(numbers) - 1  # End pointer

        while left < right:
            sum = numbers[left] + numbers[right]
            if sum == target:
                return [left + 1, right + 1]  # 1-based index
            elif sum < target:
                left += 1  # Move left pointer to increase sum
            else:
                right -= 1  # Move right pointer to decrease sum
        return [-1, -1]  # No solution found
```

