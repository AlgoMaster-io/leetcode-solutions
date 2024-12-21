# [Leetcode 233: Number of Digit One](https://leetcode.com/problems/number-of-digit-one/)

## Approaches:
- [Brute Force Approach](#brute-force-approach)
- [Digit-by-Digit Processing Approach](#digit-by-digit-processing-approach)
- [Analytical Counting Approach](#analytical-counting-approach)

---

### Brute Force Approach

The naive method to solve this problem is to iterate over each number from `1` to `n` and count how many times the digit `1` appears by converting the numbers to strings. Although this approach is straightforward and easy to implement, it is not efficient for large values of `n`.

#### Code:

```python
def countDigitOne(n: int) -> int:
    count = 0
    # Iterate over each number from 1 to n
    for i in range(1, n + 1):
        # Convert the number to a string to count '1's
        count += str(i).count('1')
    return count

# Example usage
n = 13
print(countDigitOne(n)) # Output: 6
```

#### Time Complexity:
- O(n * log n): Each number is converted to a string which takes O(log n) time, and we do this for every number up to `n`.

#### Space Complexity:
- O(1): The space used is constant, as we only require a counter.

---

### Digit-by-Digit Processing Approach

Instead of examining each number individually, we can consider the problem digit by digit. This involves analyzing the contribution of each digit to the total number of `1`s in each place (units, tens, hundreds, etc.). This approach involves more complex mathematical computations of how frequently the digit `1` appears in each position.

#### Code:

```python
def countDigitOne(n: int) -> int:
    count = 0
    i = 1
    while i <= n:
        # These are important variables to determine the current place value we are evaluating
        divider = i * 10
        
        # Add the number of 1s contributed by the current place
        count += (n // divider) * i + min(max(n % divider - i + 1, 0), i)
        
        # Shift the place value
        i *= 10
    return count

# Example usage
n = 13
print(countDigitOne(n)) # Output: 6
```

#### Time Complexity:
- O(log n): We iterate through the number based on the number of digits, which is logarithmic with respect to `n`.

#### Space Complexity:
- O(1): Only a few variables are used regardless of the input size.

---

### Analytical Counting Approach

The most optimal solution involves analytical calculations without iteration by using combinatorics or mathematical formulas specific to this problem. We can mathematically predict the number of 1s based on each digit's significance. This involves understanding how `1` can contribute depending on the position in the number.

- For each digit, decide how many times it contributes based on its position.
- Use modulo and integer division to dissect each place value, and aggregate the results to get the total.

This optimized logic ensures that we quickly compute the answer through calculations instead of iterative checking.

#### Code:

```python
def countDigitOne(n: int) -> int:
    count = 0
    factor = 1
    while factor <= n:
        lower_numbers = n - (n // factor) * factor
        current_digit = (n // factor) % 10
        higher_numbers = n // (factor * 10)
        
        if current_digit == 0:
            count += higher_numbers * factor
        elif current_digit == 1:
            count += higher_numbers * factor + lower_numbers + 1
        else:
            count += (higher_numbers + 1) * factor
        
        factor *= 10
    return count

# Example usage
n = 13
print(countDigitOne(n)) # Output: 6
```

#### Time Complexity:
- O(log n): Analyzing each digit's contribution based on its place value takes logarithmic time with respect to how many digits `n` has.

#### Space Complexity:
- O(1): Only constant space is used beyond the input integer size.

--- 

These approaches provide a diverse set of solutions from straightforward iteration to complex mathematical manipulation, each increasing in efficiency.

