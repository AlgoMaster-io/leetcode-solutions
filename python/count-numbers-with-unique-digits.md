[Leetcode 357: Count Numbers with Unique Digits](https://leetcode.com/problems/count-numbers-with-unique-digits/)

## Approaches
1. [Backtracking Approach](#backtracking-approach)
2. [Mathematical Approach](#mathematical-approach)
3. [Optimized Mathematical Approach](#optimized-mathematical-approach)

---

### Backtracking Approach

#### Intuition
The basic idea is to use a backtracking technique to explore all possible combinations of digits, checking at each step whether a number formed with those digits is unique (i.e., no digits are repeated). This is a brute-force approach exploring all possibilities through recursion.

#### Implementation
```python
def countNumbersWithUniqueDigits_Backtracking(n: int) -> int:
    # Helper function to perform the backtracking
    def backtrack(number, used_digits):
        # Base case: if the length of the number exceeds n, stop recursion
        if len(number) > n:
            return 0
        # Start with counting the current number as a unique number
        count = 1
        for digit in range(0 if number else 1, 10):
            if digit not in used_digits:
                used_digits.add(digit)
                # Recurse by adding the current digit to the number
                count += backtrack(number + str(digit), used_digits)
                # Backtrack by removing the digit
                used_digits.remove(digit)
        return count

    return backtrack("", set())

# Time Complexity: O(10!) because of the factorial growth from choosing digits without replacement.
# Space Complexity: O(n) due to the recursion stack.
```

### Mathematical Approach

#### Intuition
A mathematical approach revolves around realizing that each position in the number can have a unique digit. For instance, for n = 2, the first digit can be from 1-9, and the second from 0-9 excluding the first digit; we do this iteratively while tracking the possibilities.

#### Implementation
```python
def countNumbersWithUniqueDigits_Math(n: int) -> int:
    # If n is 0, only the number 0 is possible
    if n == 0:
        return 1
    
    # Count for n = 0
    count = 1  # to include number 0
    unique_digit_count = 9  # first digit choice (can't be zero if we have more than one digit)
    current_product = 1
    
    for i in range(1, n + 1):
        if i == 1:
            current_product *= unique_digit_count
        else:
            current_product *= (10 - i + 1)
        count += current_product
    
    return count

# Time Complexity: O(n) due to the iteration from 1 to n.
# Space Complexity: O(1) as no extra space is used.
```

### Optimized Mathematical Approach

#### Intuition
We further optimize by using direct mathematical formulas. We understand that for n > 10, the result will be the same as for n = 10 because we run out of available digits.

#### Implementation
```python
def countNumbersWithUniqueDigits_OptimizedMath(n: int) -> int:
    if n == 0:
        return 1
    
    # Limit n to at most 10 since a longer number would not have unique digits
    n = min(n, 10)
    count = 10  # for n = 0,1 which includes all single-digit numbers
    unique_digit_count = 9
    current_product = 9

    for i in range(2, n + 1):
        current_product *= unique_digit_count
        count += current_product
        unique_digit_count -= 1

    return count

# Time Complexity: O(1) because n is limited to 10 iterations.
# Space Complexity: O(1) as only constant space is used.
```

Each of these approaches provides a different route to solve the problem with increasing efficiency, from exploring all possibilities with recursion to utilizing simple arithmetic calculations.

