# [132 Pattern - Leetcode 456](https://leetcode.com/problems/132-pattern/)

## Approaches

1. [Brute Force Approach](#brute-force-approach)
2. [Using Min Array and Binary Search](#using-min-array-and-binary-search)
3. [Optimal Approach using Stack](#optimal-approach-using-stack)

---

## Brute Force Approach

The brute force approach involves a straightforward examination of all possible triplets in the array to determine whether they form the `132 pattern`.

### Intuition

The idea is to iterate over all possible combinations of three indices such that `i < j < k` and check if the values at these indices satisfy the condition `nums[i] < nums[k] < nums[j]`.

### Implementation

```python
def find132pattern(nums):
    n = len(nums)

    # Traverse each triplet combination
    for i in range(n - 2):
        for j in range(i + 1, n - 1):
            for k in range(j + 1, n):
                # Check if it's 132 pattern
                if nums[i] < nums[k] < nums[j]:
                    return True
    return False
```

### Time and Space Complexity

- **Time Complexity:** \(O(n^3)\) - Due to three nested loops iterating over the list.
- **Space Complexity:** \(O(1)\) - No extra data structures are used.

---

## Using Min Array and Binary Search

This approach reduces the time complexity by maintaining an array of minimum values up to each index and using binary search to find a potential `nums[k]`.

### Intuition

1. Maintain a `min_array` where `min_array[i]` is the minimum value from the start to the current index `i`.
2. For each index `j`, we need to find an index `k` such that `nums[k]` is greater than `min_array[j]` and less than `nums[j]`.

### Implementation

```python
from bisect import bisect_left

def find132pattern(nums):
    n = len(nums)
    if n < 3:
        return False
    
    min_array = [nums[0]] * n
    for i in range(1, n):
        min_array[i] = min(min_array[i-1], nums[i])
    
    sorted_list = []

    for j in range(n-1, -1, -1):
        if nums[j] > min_array[j]:
            pos = bisect_left(sorted_list, min_array[j] + 1)
            if pos < len(sorted_list) and sorted_list[pos] < nums[j]:
                return True
            bisect.insort(sorted_list, nums[j])
    
    return False
```

### Time and Space Complexity

- **Time Complexity:** \(O(n \log n)\) - Due to the use of binary search for potential candidates.
- **Space Complexity:** \(O(n)\) - For the `min_array` and the sorted list.

---

## Optimal Approach using Stack

The most efficient way utilizes a stack to keep track of potential `nums[k]` from the end to the start.

### Intuition

We move backwards from the end of the array and use a stack to track all possible values of `nums[k]`. As we iterate each number as a potential `nums[j]`, we check the condition against a maintained "second" number, which acts as our `nums[k]`.

1. Use a stack to maintain potential `nums[k]`.
2. Traverse the list from the right to find feasible `nums[j]` and `nums[k]`.

### Implementation

```python
def find132pattern(nums):
    stack = []
    second = float('-inf')

    for num in reversed(nums):
        # Check if the current number can be nums[i]
        if num < second:
            return True
        # Update the stack with potential nums[k]
        while stack and num > stack[-1]:
            second = stack.pop()
        stack.append(num)

    return False
```

### Time and Space Complexity

- **Time Complexity:** \(O(n)\) - Each element is pushed and popped from the stack at most once.
- **Space Complexity:** \(O(n)\) - The stack can hold up to `n` elements.

