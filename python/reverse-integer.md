[Leetcode Problem #7: Reverse Integer](https://leetcode.com/problems/reverse-integer/)

In this problem, you must reverse the digits of a 32-bit signed integer. The trick is handling the overflow and maintaining the sign of the integer. If the integer overflows when reversed, you should return 0.

### Approaches:
1. [Basic Approach: Reverse as String](#basic-approach-reverse-as-string)
2. [Intermediate Approach: Mathematical Reversion](#intermediate-approach-mathematical-reversion)
3. [Optimal Approach: Direct Reversion with Overflow Check](#optimal-approach-direct-reversion-with-overflow-check)

---

### Basic Approach: Reverse as String

#### Intuition:
Simply convert the integer to a string and reverse it. Handle the sign separately and take care of overflow by handling the result after conversion.

```python
def reverse(x: int) -> int:
    # Convert integer to a string
    str_x = str(x)
    # If the number is negative, reverse the substring starting from index 1 (excluding the negative sign)
    if str_x[0] == '-':
        reversed_x = int('-' + str_x[1:][::-1])
    else:
        # Reverse the string and convert back to int
        reversed_x = int(str_x[::-1])
    # Check for 32-bit signed integer overflow
    if reversed_x < -2**31 or reversed_x > 2**31 - 1:
        return 0
    return reversed_x
```

**Time Complexity**: O(n), where n is the number of digits in the integer (converts and reverses a string).  
**Space Complexity**: O(n), as we create a new string to store the reversed digits.

---

### Intermediate Approach: Mathematical Reversion

#### Intuition:
Extract digits one by one from the end and build the reverse number using mathematical operations. Compare the intermediate results to check for overflow.

```python
def reverse(x: int) -> int:
    INT_MIN, INT_MAX = -2**31, 2**31 - 1
    result = 0

    while x != 0:
        # Extract the last digit
        digit = int(x % 10)
        # Manage negative numbers
        if x < 0 and digit > 0:
            digit -= 10
        x = int(x / 10)

        # Handle overflow
        if result > INT_MAX // 10 or (result == INT_MAX // 10 and digit > INT_MAX % 10):
            return 0
        if result < INT_MIN // 10 or (result == INT_MIN // 10 and digit < INT_MIN % 10):
            return 0

        # Add digit to reversed number
        result = result * 10 + digit

    return result
```

**Time Complexity**: O(log(x)), with x being the value of the input. We process each digit once.  
**Space Complexity**: O(1), as no additional space proportional to input size is used.

---

### Optimal Approach: Direct Reversion with Overflow Check

#### Intuition:
It combines digit extraction and result formation with the most optimized approach to check overflow conditions carefully, managing sign and handling efficiently.

```python
def reverse(x: int) -> int:
    INT_MIN, INT_MAX = -2**31, 2**31 - 1
    result = 0

    while x != 0:
        # Pop operation: Extract the last digit
        digit = x % 10
        if x < 0 and digit > 0:
           digit -= 10
        x = int(x / 10) 

        # Overflow check before pushing to result
        if result > INT_MAX // 10 or (result == INT_MAX // 10 and digit > INT_MAX % 10):
            return 0
        if result < INT_MIN // 10 or (result == INT_MIN // 10 and digit < INT_MIN % 10):
            return 0

        # Push operation: Append the popped digit to the result
        result = result * 10 + digit

    return result
```

**Time Complexity**: O(log(x)), with x being the value of the input. Each digit is processed once.  
**Space Complexity**: O(1), as we don't utilize extra space proportional to the input size.

