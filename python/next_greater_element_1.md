# [496. Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/)

## Approach 1: Brute Force

### Solution
```python
# Time Complexity: O(n * m)
# Space Complexity: O(1)
class Solution:
    def nextGreaterElement(self, nums1, nums2):
        result = [-1] * len(nums1)

        # Iterate over each element in nums1
        for i in range(len(nums1)):
            current = nums1[i]
            index_in_nums2 = -1

            # Find the index of current in nums2
            for j in range(len(nums2)):
                if nums2[j] == current:
                    index_in_nums2 = j
                    break

            # Look for the next greater element to the right in nums2
            for j in range(index_in_nums2 + 1, len(nums2)):
                if nums2[j] > current:
                    result[i] = nums2[j]
                    break

        return result
```

## Approach 2: Using a Stack (Optimal Solution)

### Solution
```python
# Time Complexity: O(n + m)
# Space Complexity: O(m)
class Solution:
    def nextGreaterElement(self, nums1, nums2):
        # Map to store the next greater element for each number in nums2
        next_greater_map = {}
        stack = []

        # Traverse nums2 in reverse to populate the next_greater_map
        for num in nums2:
            # Maintain the stack in decreasing order
            while stack and stack[-1] <= num:
                stack.pop()
            # If stack is not empty, the top of the stack is the next greater element
            next_greater_map[num] = stack[-1] if stack else -1
            stack.append(num)

        # Build the result for nums1 using the map
        result = [next_greater_map[num] for num in nums1]

        return result
```

