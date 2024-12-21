## [Leetcode 7: Reverse Integer](https://leetcode.com/problems/reverse-integer/)

### Approaches:
1. [String Conversion](#string-conversion)
2. [Mathematical Reversal](#mathematical-reversal)

### Solution 1: String Conversion

#### Intuition:
The simplest way to reverse an integer is by converting it into a string, reversing the string, and converting it back. This approach is straightforward and leverages JavaScript's built-in methods for string manipulation. However, it is not the most efficient.

#### Implementation:
```javascript
function reverse(x) {
    // Define the limit for 32-bit signed integers
    const limit = 2**31;
    // Determine if the number is negative
    const sign = x < 0 ? -1 : 1;
    // Convert the number to a string, reverse it, and parse to integer
    const reversed = parseInt(Math.abs(x).toString().split('').reverse().join('')) * sign;
    // Check if the reversed integer is within the 32-bit signed integer range
    if (reversed < -limit || reversed > limit - 1) {
        return 0;
    }
    // Return the valid reversed integer
    return reversed;
}
```

#### Time Complexity:
- **O(n)**, where n is the number of digits in `x`. We iterate over the digits when converting to string, reversing, and joining.

#### Space Complexity:
- **O(n)**, due to the storage requirement for the string and array representation of the number.

### Solution 2: Mathematical Reversal

#### Intuition:
To make the solution more optimal, we can avoid string manipulation altogether by manually reversing the integer. We can extract the last digit of the number using modulo operation and build the reversed number step by step. This approach handles the numbers as numerical values directly.

#### Implementation:
```javascript
function reverse(x) {
    // Define the limit for 32-bit signed integers
    const limit = 2**31;
    let result = 0;
    let num = x;
    
    while (num !== 0) {
        // Extract the last digit of the number
        const digit = num % 10;
        // Prepare for the next loop iteration by removing the last digit
        num = parseInt(num / 10);
        // Check for overflow before adding the new digit
        if (result > (limit - 1) / 10 || result < (-limit / 10)) {
            return 0;
        }
        // Add the extracted digit to the reversed result
        result = result * 10 + digit;
    }
    return result;
}
```

#### Time Complexity:
- **O(n)**, where n is the number of digits in `x`. Each loop iteration processes one digit.

#### Space Complexity:
- **O(1)**, since we are only using a fixed amount of additional space regardless of the input size.

By using the mathematical approach, we achieve an efficient and optimal solution that handles integer reversal without unnecessary conversion overhead. This method also naturally handles edge cases related to integer overflow.

