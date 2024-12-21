# [Leetcode 9: Palindrome Number](https://leetcode.com/problems/palindrome-number/)

## Approaches
- [Approach 1: Convert to String](#approach-1-convert-to-string)
- [Approach 2: Reverse Half of the Number](#approach-2-reverse-half-of-the-number)

## Approach 1: Convert to String

### Intuition
The most straightforward method to determine if a number is a palindrome is by converting the number into a string. A palindrome remains the same when read forwards and backwards. Hence, by transforming the number into a string, we can easily compare the string to its reverse.

### Solution
```javascript
/**
 * @param {number} x
 * @return {boolean}
 */
var isPalindrome = function(x) {
    // Negative numbers are not palindrome
    if (x < 0) return false;
    
    // Convert number to string
    let str = x.toString();
    
    // Reverse the string
    let reverseStr = str.split('').reverse().join('');
    
    // Compare the original string with the reversed string
    return str === reverseStr;
};
```
### Time Complexity
- O(n), where n is the number of digits in the number. We convert the number to a string and then reverse it.
  
### Space Complexity
- O(n), where n is the number of digits in the number. This is due to storing the number as a string and its reverse.

## Approach 2: Reverse Half of the Number

### Intuition
Instead of reversing the entire number, we can check for a palindrome by reversing only half of the digits. The main idea is that for a palindrome, the first half of digits should equal the reverse of the second half of digits. This reduces unnecessary calculations.

### Solution
```javascript
/**
 * @param {number} x
 * @return {boolean}
 */
var isPalindrome = function(x) {
    // Negative numbers or numbers ending with 0 (except 0 itself) can't be palindromes
    if (x < 0 || (x % 10 === 0 && x !== 0)) return false;
    
    let reversedNumber = 0;
    
    // Reverse the second half of the number
    while (x > reversedNumber) {
        reversedNumber = reversedNumber * 10 + x % 10; // Add the last digit of x to reversedNumber
        x = Math.floor(x / 10);                        // Remove the last digit from x
    }
    
    // When numbers have odd number of digits, ignore the middle digit by reversedNumber/10
    return x === reversedNumber || x === Math.floor(reversedNumber / 10);
};
```

### Time Complexity
- O(log₁₀(n)), where n is the input number. We only reverse half of the number's digits.

### Space Complexity
- O(1), as we use a constant amount of extra space.

