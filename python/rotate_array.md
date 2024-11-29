# [189. Rotate Array](https://leetcode.com/problems/rotate-array/)

## Approach 1: Using Extra Array

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
class Solution:
    def rotate(self, nums, k):
        n = len(nums)
        k = k % n  # Handle cases where k > n
        rotated = [0] * n

        # Copy rotated elements to a new array
        for i in range(n):
            rotated[(i + k) % n] = nums[i]

        # Copy elements back to the original array
        for i in range(n):
            nums[i] = rotated[i]
```

## Approach 2: Rotate One-by-One (Brute Force)

### Solution
```python
# Time Complexity: O(n * k)
# Space Complexity: O(1)
class Solution:
    def rotate(self, nums, k):
        n = len(nums)
        k = k % n  # Handle cases where k > n

        # Rotate the array k times
        for _ in range(k):
            last = nums[n - 1]  # Save the last element
            # Shift elements to the right
            for j in range(n - 1, 0, -1):
                nums[j] = nums[j - 1]
            nums[0] = last  # Place the saved element at the beginning
```

## Approach 3: Reverse Array (Optimal)

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def rotate(self, nums, k):
        n = len(nums)
        k = k % n  # Handle cases where k > n

        # Step 1: Reverse the entire array
        self.reverse(nums, 0, n - 1)

        # Step 2: Reverse the first k elements
        self.reverse(nums, 0, k - 1)

        # Step 3: Reverse the remaining n - k elements
        self.reverse(nums, k, n - 1)

    def reverse(self, nums, start, end):
        while start < end:
            nums[start], nums[end] = nums[end], nums[start]
            start, end = start + 1, end - 1
```

## Approach 4: Cyclic Replacements

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def rotate(self, nums, k):
        n = len(nums)
        k = k % n  # Handle cases where k > n
        count = 0  # Number of elements rotated

        for start in range(n):
            if count >= n:
                break
            current = start
            prev = nums[start]

            while True:
                next_idx = (current + k) % n
                nums[next_idx], prev = prev, nums[next_idx]
                current = next_idx
                count += 1

                if start == current:
                    break
```

