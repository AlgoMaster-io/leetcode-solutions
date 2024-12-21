# [Leetcode 1004: Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/)

### Solutions:

1. [Basic Approach: Brute Force](#brute-force)
2. [Sliding Window Approach](#sliding-window)
3. [Optimized Sliding Window Approach](#optimized-sliding-window)

---

## Basic Approach: Brute Force

### Intuition:
The simplest approach is to consider every subarray of the input array and calculate how many 0s can be flipped to 1s within that subarray. For subarrays that have a number of 0s less than or equal to `k`, calculate the length of such subarray and try to track the maximum length.

### Time Complexity:
- **O(n^2)**: We are traversing a subarray for every possible starting point.
  
### Space Complexity:
- **O(1)**: Only a fixed number of variables are used.

### Python Code:
```python
def longestOnes(A, K):
    max_consecutive_ones = 0
    n = len(A)
    
    for start in range(n):
        zeros_count = 0
        for end in range(start, n):
            if A[end] == 0:
                zeros_count += 1
            if zeros_count <= K:
                max_consecutive_ones = max(max_consecutive_ones, end - start + 1)
            else:
                break

    return max_consecutive_ones
```

---

## Sliding Window Approach

### Intuition:
Using a sliding window approach simplifies the brute force method by maintaining a window and adjusting its size as conditions are met. Start with both window edges at the beginning of the array. Expand the window by moving the end edge when a 1 is encountered. If a 0 is encountered, decrement K if it's permissible (K > 0), otherwise, shrink the window from the start.

### Time Complexity:
- **O(n)**: We traverse the input array `A` once with the sliding window.
  
### Space Complexity:
- **O(1)**: Only a fixed number of variables are used.

### Python Code:
```python
def longestOnes(A, K):
    left = 0
    for right in range(len(A)):
        # If we include a zero in the window, reduce K
        if A[right] == 0:
            K -= 1
        # If we have more zeros than we can flip, shrink the window
        if K < 0:
            # If the leftmost element was 0, increment K back
            if A[left] == 0:
                K += 1
            left += 1
            
    return right - left + 1
```

---

## Optimized Sliding Window Approach

### Intuition:
This approach improves the sliding window technique slightly by doing calculations as edges move, ensuring minimum redundant operations. We adjust only when zeros surpass `K` and move the left edge precisely without iteratively checking each element. This ensures that the window is never larger than it should be without needing excess checks.

### Time Complexity:
- **O(n)**: Every element is processed at most twice (once added to the window, once removed).
  
### Space Complexity:
- **O(1)**: Again, only a fixed number of variables used.

### Python Code:
```python
def longestOnes(A, K):
    left = right = 0
    zeros_count = 0

    while right < len(A):
        if A[right] == 0:
            zeros_count += 1
        
        if zeros_count > K:
            if A[left] == 0:
                zeros_count -= 1
            left += 1

        right += 1

    return right - left
```

This optimized sliding window method offers a simple and clear approach to handling the problem efficiently. It ensures we are always operating within our constraints, leading to a solution in linear time with constant space.

