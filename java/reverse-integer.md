# [Leetcode 7: Reverse Integer](https://leetcode.com/problems/reverse-integer/)

## Approaches:
1. [String Manipulation](#approach-1-string-manipulation)
2. [Mathematical Reverse](#approach-2-mathematical-reverse)
3. [Optimized Mathematical Reverse with Overflow Check](#approach-3-optimized-mathematical-reverse-with-overflow-check)

## Approach 1: String Manipulation
This simple approach involves converting the integer to a string, reversing it and converting it back to an integer. While naive, it's straightforward to implement and understand.

### Code:
```java
public class Solution {
    public int reverse(int x) {
        // Convert the integer to a string
        StringBuilder sb = new StringBuilder(Integer.toString(x));
        
        // If the integer is negative, reverse the string except the '-' sign
        if (x < 0) {
            sb.deleteCharAt(0); // remove the '-' for reversal
            sb.reverse();
            sb.insert(0, '-'); // add '-' back after reversal
        } else {
            sb.reverse();
        }

        // Try to convert the reversed string back to an integer
        try {
            return Integer.parseInt(sb.toString());
        } catch (NumberFormatException e) {
            // If any exception occurs due to overflow of integer range, return 0
            return 0;
        }
    }
}
```
### Complexity Analysis:
- **Time Complexity**: O(n), where n is the number of digits in the integer. This results from reversing the string representation of the integer.
- **Space Complexity**: O(1) ignoring the space used for input and output as this uses a constant amount of space for the StringBuilder operations.

## Approach 2: Mathematical Reverse
Instead of using built-in string manipulation, this approach repeatedly divides the input by 10 to extract digits and form the reversed integer.

### Code:
```java
public class Solution {
    public int reverse(int x) {
        int result = 0;
        while (x != 0) {
            // Pop the last digit from x
            int pop = x % 10;
            x /= 10;

            // Form the reversed number
            result = result * 10 + pop;
        }
        return result;
    }
}
```
### Complexity Analysis:
- **Time Complexity**: O(log(x)), where x is the input integer. We extract each digit one by one and combine it in reversed order.
- **Space Complexity**: O(1), since we're using a fixed amount of extra space.

## Approach 3: Optimized Mathematical Reverse with Overflow Check
Handling overflow is crucial because the problem constraints specify that the reversed integer must fit within a 32-bit signed integer range. This can be efficiently achieved by checking possible overflow conditions before updating the result in each iteration.

### Code:
```java
public class Solution {
    public int reverse(int x) {
        int result = 0;
        int maxDiv10 = Integer.MAX_VALUE / 10;
        int minDiv10 = Integer.MIN_VALUE / 10;
        
        while (x != 0) {
            // Pop the last digit
            int pop = x % 10;
            x /= 10;

            // Check for overflow before updating the result
            if (result > maxDiv10 || (result == maxDiv10 && pop > 7)) return 0;
            if (result < minDiv10 || (result == minDiv10 && pop < -8)) return 0;

            // Add the popped digit into the reversed number
            result = result * 10 + pop;
        }
        return result;
    }
}
```
### Complexity Analysis:
- **Time Complexity**: O(log(x)), similar to the previous mathematical approach, handling each digit once.
- **Space Complexity**: O(1), as we use a minimal, constant amount of additional space for variables.

By following these approaches, you can achieve success in reversing an integer under constraints using different methods, from basic to optimal.

