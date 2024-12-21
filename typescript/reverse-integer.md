## Link to the Problem
[Reverse Integer - LeetCode](https://leetcode.com/problems/reverse-integer/)

## Approaches
1. [Basic String Conversion](#basic-string-conversion)
2. [Mathematical Approach](#mathematical-approach)

### 1. Basic String Conversion

**Intuition**:  
The simplest way to reverse an integer is to convert it to a string, reverse the string, and then convert it back to an integer. The challenge, however, is to handle negative numbers correctly and to check for integer overflow.

**Solution**:  
1. Convert the integer to a string.
2. Check for a negative sign. If it's negative, retain the '-' after reversing.
3. Reverse the string.
4. Convert back to integer.
5. Handle overflow by checking if the reversed integer fits within the 32-bit signed integer range.

**Time Complexity**: O(n) - where n is the number of digits in the number, primarily for string reversal.  
**Space Complexity**: O(n) - for storing the string representation of the number.

```typescript
function reverseIntegerString(x: number): number {
    // Handle the edge case of 0 immediately
    if (x === 0) return 0;

    // Convert the number to a string
    const str = x.toString();

    // Check if the number is negative and reverse accordingly
    let reversedStr;
    if (x < 0) {
        reversedStr = '-' + str.slice(1).split('').reverse().join('');
    } else {
        reversedStr = str.split('').reverse().join('');
    }

    // Parse the reversed string back to an integer
    const reversedInteger = parseInt(reversedStr, 10);

    // Return 0 if the reversed integer overflows 32-bit integer range
    if (reversedInteger < -(2**31) || reversedInteger > (2**31 - 1)) {
        return 0;
    }

    return reversedInteger;
}
```

### 2. Mathematical Approach

**Intuition**:  
Instead of converting the number to a string, we can use arithmetic operations to reverse the number. This approach avoids the need for string manipulation and directly handles the integer operations. The idea is to pop the last digit from the number and push it to the reversed number.

**Solution**:  
1. Initialize a variable `reversed` to store the reversed number.
2. Loop while the number is not zero:
   - Pop the last digit (`pop = x % 10`).
   - Check for overflow before multiplying `reversed` by 10 or adding `pop`.
   - Push the popped digit into `reversed` by `reversed = reversed * 10 + pop`.
   - Reduce `x` by `x = Math.trunc(x / 10)`.
3. Ensure the reversed integer fits the 32-bit signed integer range.

**Time Complexity**: O(log(x)) - x is reduced by a factor of 10 every iteration.  
**Space Complexity**: O(1) - no extra space is used.

```typescript
function reverseIntegerMath(x: number): number {
    let reversed = 0;

    while (x !== 0) {
        // Pop the last digit
        const pop = x % 10;
        x = Math.trunc(x / 10);

        // Check for overflow before modifying reversed
        if (reversed > (2**31 - 1) / 10 || (reversed === (2**31 - 1) / 10 && pop > 7)) {
            return 0;
        }
        if (reversed < -(2**31) / 10 || (reversed === -(2**31) / 10 && pop < -8)) {
            return 0;
        }

        // Push to result
        reversed = reversed * 10 + pop;
    }

    return reversed;
}
```

Here, the mathematical approach is more optimal as it doesn't require extra space for strings and handles integer operations directly, thus making it a preferred solution for large inputs.

