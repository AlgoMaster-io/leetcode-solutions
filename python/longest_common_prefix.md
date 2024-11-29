# 14. [Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)

## Approach 1: Horizontal Scanning

### Solution
python
```python
# Time Complexity: O(S) where S is the sum of all characters in the strings
# Space Complexity: O(1)
class Solution:
    def longestCommonPrefix(self, strs):
        if not strs:
            return ""

        # Start with the first string as the prefix
        prefix = strs[0]

        # Compare the prefix with each string in the array
        for i in range(1, len(strs)):
            while strs[i].find(prefix) != 0:
                # Shorten the prefix until it matches the start of the current string
                prefix = prefix[:-1]
                if not prefix:
                    return ""  # No common prefix

        return prefix
```

## Approach 2: Vertical Scanning

### Solution
python
```python
# Time Complexity: O(S) where S is the sum of all characters in the strings
# Space Complexity: O(1)
class Solution:
    def longestCommonPrefix(self, strs):
        if not strs:
            return ""

        # Iterate over each character of the first string
        for i in range(len(strs[0])):
            c = strs[0][i]

            # Compare this character with the same position in all strings
            for j in range(1, len(strs)):
                # If characters don't match or exceed the string's length
                if i == len(strs[j]) or strs[j][i] != c:
                    return strs[0][:i]  # Return the common prefix

        return strs[0]  # The entire first string is the common prefix
```

## Approach 3: Divide and Conquer

### Solution
python
```python
# Time Complexity: O(S) where S is the sum of all characters in the strings
# Space Complexity: O(m * log n) where m is the average string length
class Solution:
    def longestCommonPrefix(self, strs):
        if not strs:
            return ""

        def longestCommonPrefix(strs, left, right):
            if left == right:
                return strs[left]  # Base case: single string

            # Divide the array into two halves
            mid = (left + right) // 2
            lcpLeft = longestCommonPrefix(strs, left, mid)
            lcpRight = longestCommonPrefix(strs, mid + 1, right)

            # Merge results
            return commonPrefix(lcpLeft, lcpRight)

        def commonPrefix(left, right):
            minLength = min(len(left), len(right))
            for i in range(minLength):
                if left[i] != right[i]:
                    return left[:i]
            return left[:minLength]

        return longestCommonPrefix(strs, 0, len(strs) - 1)
```

## Approach 4: Binary Search

### Solution
python
```python
# Time Complexity: O(S * log m) where S is the sum of characters in strings and m is the minimum string length
# Space Complexity: O(1)
class Solution:
    def longestCommonPrefix(self, strs):
        if not strs:
            return ""

        minLen = min(len(s) for s in strs)

        low, high = 1, minLen
        while low <= high:
            mid = (low + high) // 2
            if self.isCommonPrefix(strs, mid):
                low = mid + 1  # Try for a longer prefix
            else:
                high = mid - 1  # Try for a shorter prefix

        return strs[0][:(low + high) // 2]

    def isCommonPrefix(self, strs, length):
        prefix = strs[0][:length]
        for i in range(1, len(strs)):
            if not strs[i].startswith(prefix):
                return False
        return True
```

