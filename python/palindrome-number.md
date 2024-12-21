# [Leetcode Problem 9: Palindrome Number](https://leetcode.com/problems/palindrome-number/)

## Approaches:
1. [Reverse the Number](#reverse-the-number)
2. [Compare with Half Reversed Number](#compare-with-half-reversed-number)

### Reverse the Number

**Intuition:**

The simplest way to determine if a number is a palindrome is to reverse the digits of the number and check if the reversed number is equal to the original number. If they are the same, the number is a palindrome.

**Steps:**

1. Handle special cases: a negative number or any number ending in zero that is not zero itself cannot be a palindrome.
2. Reverse the digit of the number and store it in another variable.
3. Compare the original number with the reversed number.

**Code:**

```python
def isPalindrome(x: int) -> bool:
    # A negative number or a number that ends in 0 is not a palindrome,
    # unless the number is 0 itself.
    if x < 0 or (x % 10 == 0 and x != 0):
        return False
    
    reversed_num = 0
    original = x
    
    # Reverse the number
    while x > 0:
        # Take the last digit of x
        last_digit = x % 10
        # Append last digit to reversed_num
        reversed_num = reversed_num * 10 + last_digit
        # Remove last digit from x
        x = x // 10
    
    # Check if the original number equals the reversed number.
    return original == reversed_num
```

**Time Complexity:** O(n), where n is the number of digits in the number.

**Space Complexity:** O(1), as we are using a constant amount of space.

### Compare with Half Reversed Number

**Intuition:**

To optimize, we can avoid reversing the entire number by only reversing half of it. We can obtain the reversed number of the last half of the digits, then compare it with the first half.

**Steps:**

1. Handle the same special cases as before: a negative number or any number ending in zero that is not zero itself cannot be a palindrome.
2. Initialize a variable to store the half-reversed part of the number.
3. Keep building the half-reversed number by taking digits from the end of the original number.
4. Stop the process when the half-reversed number is greater than or equal to the original number. This indicates that we've processed half (or more) of the digits.
5. Compare the original number (first half) with the half-reversed number, or ignore the middle digit if the number of digits is odd.

**Code:**

```python
def isPalindrome(x: int) -> bool:
    # A negative number or a number that ends in 0 is not a palindrome,
    # unless the number is 0 itself.
    if x < 0 or (x % 10 == 0 and x != 0):
        return False
    
    reversed_half = 0
    
    # Reverse the digits until the reversed number is at least half of the original
    while x > reversed_half:
        # Append last digit to reversed_half
        reversed_half = reversed_half * 10 + x % 10
        # Remove last digit from x
        x //= 10
    
    # Check if the reversed_half is equal to x (even-length number case)
    # or reversed_half // 10 is equal to x (odd-length number case)
    return x == reversed_half or x == reversed_half // 10
```

**Time Complexity:** O(log10(n)), the number of digits in the input number.

**Space Complexity:** O(1), as we are using a constant amount of space.

