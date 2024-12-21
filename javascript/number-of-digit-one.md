# [Leetcode 233: Number of Digit One](https://leetcode.com/problems/number-of-digit-one/)

## Table of Contents
- [Approach 1: Brute Force Enumeration](#approach-1-brute-force-enumeration)
- [Approach 2: Mathematical Counting](#approach-2-mathematical-counting)

### Approach 1: Brute Force Enumeration
This approach involves checking every number from 1 to `n` and counting the '1's in each number. Although inefficient for large `n`, it serves as an introductory solution.

#### Intuition
We iterate from 1 to `n` and for each number, count the occurrences of the digit '1'. This is done by converting the number to a string and using string manipulation techniques.

#### Implementation
```javascript
function countDigitOne(n) {
    let count = 0;
    // Iterate through each number from 1 to n
    for (let i = 1; i <= n; i++) {
        // Convert the number to a string and split into individual characters
        const str = i.toString();
        // Count occurrences of '1' in the string
        for (let ch of str) {
            if (ch === '1') {
                count++;
            }
        }
    }
    return count;
}
```
#### Complexity Analysis
- **Time Complexity:** O(n * d), where `d` is the number of digits in `n`. This is because for each number we convert it into a string which is `O(d)`.
- **Space Complexity:** O(1), apart from the input and integer conversion.

### Approach 2: Mathematical Counting
A more efficient and optimal approach involves calculating the number of '1's occurring at each digit position mathematically without iterating through every number.

#### Intuition
For each digit position, we calculate how many times '1' would appear if we fix '1' at that position and vary the rest of the number. This can be broken down into three parts:
1. The value of the digit at the current position.
2. Higher digits which determine repetition patterns.
3. Lower digits within repetitions.

#### Implementation
```javascript
function countDigitOne(n) {
    let count = 0;
    let factor = 1;
    let lowerDigits = 0;
    
    while (n / factor > 0) {
        // Extract the current digit
        const currentDigit = Math.floor((n / factor) % 10);
        // Get the higher part of the current number
        const higherDigits = Math.floor(n / (factor * 10));
        // Get the lower part of the current number
        const lowerDigits = n - (n / factor) * factor;
        
        if (currentDigit === 0) {
            count += higherDigits * factor;
        } else if (currentDigit === 1) {
            count += higherDigits * factor + lowerDigits + 1;
        } else {
            count += (higherDigits + 1) * factor;
        }
        
        // Move to the next higher digit position
        factor *= 10;
    }
    
    return count;
}
```
#### Complexity Analysis
- **Time Complexity:** O(log n), because we only need to analyze each digit position once.
- **Space Complexity:** O(1), as we're only using a few extra variables to keep track of counts and factor. 

This approach efficiently computes the number of times the digit '1' appears in all numbers less than or equal to `n` using careful mathematical observation and digit manipulation.


