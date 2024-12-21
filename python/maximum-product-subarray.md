# [Leetcode 152: Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)

## Table of Contents
1. [Approach 1: Brute Force](#approach-1-brute-force)
2. [Approach 2: Dynamic Programming](#approach-2-dynamic-programming)
3. [Approach 3: Optimized Dynamic Programming](#approach-3-optimized-dynamic-programming)

---

## Approach 1: Brute Force

### Intuition
The brute force approach involves generating all possible subarrays and calculating the product of each. We keep track of the maximum product encountered. While simple, this approach is inefficient, especially for large input sizes.

### Implementation

```python
def maxProduct(nums):
    max_product = float('-inf')
    n = len(nums)

    # Consider every subarray starting with i and ending at j
    for i in range(n):
        current_product = 1
        for j in range(i, n):
            current_product *= nums[j]
            max_product = max(max_product, current_product)

    return max_product
```

### Complexity Analysis
- **Time Complexity**: O(N^2) - We consider all subarrays, resulting in two nested loops.
- **Space Complexity**: O(1) - No extra space is used.

---

## Approach 2: Dynamic Programming

### Intuition
In this approach, we use two auxiliary arrays, `dp_min` and `dp_max`, where `dp_min[i]` stores the minimum product of a subarray ending at `i`, and `dp_max[i]` stores the maximum product. These arrays help maintain the product of subarrays even in the presence of negative numbers, which can change the product significantly.

### Implementation

```python
def maxProduct(nums):
    n = len(nums)
    if n == 0:
        return 0

    dp_min = [0] * n
    dp_max = [0] * n

    dp_min[0] = nums[0]
    dp_max[0] = nums[0]
    max_product = nums[0]

    for i in range(1, n):
        if nums[i] >= 0:
            dp_max[i] = max(nums[i], dp_max[i-1] * nums[i])
            dp_min[i] = min(nums[i], dp_min[i-1] * nums[i])
        else:
            dp_max[i] = max(nums[i], dp_min[i-1] * nums[i])
            dp_min[i] = min(nums[i], dp_max[i-1] * nums[i])
        
        max_product = max(max_product, dp_max[i])
    
    return max_product
```

### Complexity Analysis
- **Time Complexity**: O(N) - We traverse the array once.
- **Space Complexity**: O(N) - Due to the use of two auxiliary arrays.

---

## Approach 3: Optimized Dynamic Programming

### Intuition
We can optimize the space complexity of the dynamic programming approach by using two variables instead of two arrays. This reduces the space used while maintaining the logic of comparing maximum and minimum products at each step.

### Implementation

```python
def maxProduct(nums):
    if not nums:
        return 0

    current_max = nums[0]
    current_min = nums[0]
    global_max = nums[0]

    for num in nums[1:]:
        if num < 0:
            # Swap current_max and current_min when num is negative
            current_max, current_min = current_min, current_max

        # Update the current maximum/minimum product with the current number
        current_max = max(num, current_max * num)
        current_min = min(num, current_min * num)

        # Update the global maximum product
        global_max = max(global_max, current_max)

    return global_max
```

### Complexity Analysis
- **Time Complexity**: O(N) - We traverse the array once.
- **Space Complexity**: O(1) - Only a constant amount of space is used.

