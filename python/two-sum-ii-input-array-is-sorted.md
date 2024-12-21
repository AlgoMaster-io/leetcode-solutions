# [Leetcode 167: Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

## Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Two Pointers](#approach-2-two-pointers)

### Approach 1: Brute Force

The simplest possible solution is to use a nested loop to generate all possible pairs of numbers and check if their sum equals the target. This method is intuitive but not efficient.

**Intuition:**
- Iterate through each element of the array using two nested loops.
- For each element `numbers[i]`, check for every element `numbers[j]` (where `j > i`) if `numbers[i] + numbers[j]` equals the target.
- If such a pair is found, return their indices (converted to 1-based as required by the problem).

**Time Complexity:** `O(n^2)`, because of the nested loops.

**Space Complexity:** `O(1)`, since no extra space is used apart from simple variables.

```python
def twoSum(numbers, target):
    # Iterate over each element
    for i in range(len(numbers)):
        # Check each following element
        for j in range(i + 1, len(numbers)):
            # If the sum of the pair is the target, return indices + 1
            if numbers[i] + numbers[j] == target:
                # LeetCode requires a 1-based index result
                return [i + 1, j + 1]
```

### Approach 2: Two Pointers

Given that the array is sorted, we can use the two-pointer technique to find the solution more efficiently.

**Intuition:**
- Use two pointers: one starting at the beginning of the array (`left`) and another at the end (`right`).
- Calculate the sum of the elements at these indices.
  - If the sum equals the target, return the indices.
  - If the sum is less than the target, increment the `left` pointer to increase the sum.
  - If the sum is greater than the target, decrement the `right` pointer to decrease the sum.
- This method works optimally because as the sum can be adjusted based on the sorted nature of the array.

**Time Complexity:** `O(n)`, where `n` is the number of elements in the array. Each element is processed at most once.

**Space Complexity:** `O(1)`, since it uses a fixed number of additional variables.

```python
def twoSum(numbers, target):
    # Initialize two pointers
    left, right = 0, len(numbers) - 1
    
    while left < right:
        # Calculate current sum
        current_sum = numbers[left] + numbers[right]
        
        if current_sum == target:
            # Found the pair, return 1-based indices
            return [left + 1, right + 1]
        elif current_sum < target:
            # Increase sum by moving left pointer forward
            left += 1
        else:
            # Decrease sum by moving right pointer backward
            right -= 1
```

The two-pointer approach is more optimal, reducing the time complexity significantly, and it leverages the sorted property of the array efficiently.

