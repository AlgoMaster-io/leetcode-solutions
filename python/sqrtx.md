
# Subarray Sum Equals K
Given a non-negative integer x, compute and return the square root of x. Since the return type is an integer, the decimal digits are truncated, and only the integer part of the result is returned.

### Constraints:
- 0 <= x <= 2^31 - 1

### Examples
```javascript
Input: x = 4
Output: 2

Input: x = 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since the decimal part is truncated, 2 is returned.
```

## Approaches to Solve the Problem
### Approach 1: Brute Force (Inefficient)
##### Intuition:
One simple approach is to iterate from 1 to x and check for the largest integer i such that i * i <= x. This can be done by iterating over all numbers starting from 1 and squaring them, stopping when we find a number whose square exceeds x.

Steps:
1. Start a loop from 1 to x (or an early stopping point).
2. For each number i, check if i * i is less than or equal to x.
3. If i * i exceeds x, return i - 1 because i - 1 is the largest integer such that (i-1)² <= x.
##### Time Complexity:
O(√x), because we only iterate up to the square root of x.
##### Space Complexity:
O(1), as we only use a few variables to store the result.
##### Python Code:
```python
def mySqrt(x: int) -> int:
    if x == 0:
        return 0
    i = 1
    while i * i <= x:
        i += 1
    return i - 1
```
### Approach 2: Prefix Sum with Hash Map
##### Intuition: 
We can optimize the brute-force approach using binary search. The square root of a number x lies between 0 and x. We can apply binary search to find the integer square root. For each middle value mid, check if mid * mid is less than or equal to x, then move the search space accordingly.

Steps:
1. Set left = 0 and right = x.
2. While left <= right:
   - Calculate mid = (left + right) // 2.
   - If mid * mid == x, return mid.
   - If mid * mid < x, move the left pointer to mid + 1.
   - If mid * mid > x, move the right pointer to mid - 1.
3. If no exact match is found, return right, which represents the truncated integer square root.
##### Visualization:
For x = 8:

```perl
Initial state: left = 0, right = 8
Iteration 1: mid = 4 → 4 * 4 = 16 > 8 → move right to 3
Iteration 2: mid = 1 → 1 * 1 = 1 < 8 → move left to 2
Iteration 3: mid = 2 → 2 * 2 = 4 < 8 → move left to 3
Iteration 4: mid = 3 → 3 * 3 = 9 > 8 → move right to 2
Final result: right = 2 (largest integer such that 2 * 2 <= 8)
```
##### Time Complexity:
O(log x), because binary search divides the search space in half with each iteration.
##### Space Complexity:
O(1), because no extra space is used apart from a few variables.
##### Python Code:
```python
def mySqrt(x: int) -> int:
    if x == 0:
        return 0
    
    left, right = 0, x
    while left <= right:
        mid = (left + right) // 2
        if mid * mid == x:
            return mid
        elif mid * mid < x:
            left = mid + 1
        else:
            right = mid - 1
    
    return right
```
### Approach 3: Newton's Method (Mathematical Approach)
##### Intuition: 
Newton's method is an iterative mathematical approach that can find the square root of a number by solving the equation f(y) = y² - x. The iteration formula is:
```y = (y + x / y) / 2```

Starting with an initial guess y = x, the formula converges to the square root of x over time.

Steps:
1. Start with an initial guess y = x.
2. Use the iterative formula y = (y + x / y) / 2 to improve the guess.
3. Repeat the process until y * y is close enough to x.
##### Visualization:
For x = 8, the iterations would look like this:

```perl
Initial guess: y = 8
Iteration 1: y = (8 + 8 / 8) / 2 = 4.5
Iteration 2: y = (4.5 + 8 / 4.5) / 2 ≈ 3.1389
Iteration 3: y = (3.1389 + 8 / 3.1389) / 2 ≈ 2.843
Converges to y ≈ 2.828, and integer part is 2.
```
##### Time Complexity:
O(log x), because Newton's method converges quickly.
##### Space Complexity:
O(1), because no additional space is used.
##### Python Code:
```python
def mySqrt(x: int) -> int:
    if x == 0:
        return 0
    y = x
    while y * y > x:
        y = (y + x // y) // 2
    return y
```
### Edge Cases:
1. x = 0: The square root of 0 is 0.
2. x = 1: The square root of 1 is 1.
3. Large x: For large values of x like 2^31 - 1, the algorithm should efficiently handle large numbers and still return the correct integer result.
## Summary

| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Brute Force                    | O(√x)      | O(1)             |
| Binary Search                          | 	O(log x)            | O(1)             |
| Newton's Method                          | 	O(log x)            | O(1)             |

The Binary Search approach is the most intuitive and optimal for this problem, offering logarithmic time complexity and constant space usage. Newton's Method is a powerful mathematical technique that also solves the problem efficiently with quick convergence.