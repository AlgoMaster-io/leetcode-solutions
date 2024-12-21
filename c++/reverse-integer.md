# [Leetcode 7: Reverse Integer](https://leetcode.com/problems/reverse-integer/)

## Approaches
- [Approach 1: Basic Approach with String Conversion](#approach-1:-basic-approach-with-string-conversion)
- [Approach 2: Mathematical Approach using Modulo and Division](#approach-2:-mathematical-approach-using-modulo-and-division)

## Approach 1: Basic Approach with String Conversion

### Intuition
A straightforward way to reverse an integer is to convert the integer into a string, reverse that string, and then convert it back to an integer. This approach exploits the capabilities of high-level languages to handle string operations easily, but it is not the most efficient way as it involves additional overhead of string manipulation.

### Steps
1. Convert the integer to a string.
2. Handle the sign separately: if negative, remember the negative sign and work with the absolute value.
3. Reverse the string representation of the number.
4. Convert the reversed string back to an integer.
5. Restore the sign if necessary.
6. Check for 32-bit overflow. If the reversed integer overflows, return 0.

### Code
```cpp
class Solution {
public:
    int reverse(int x) {
        // Handle sign
        bool isNegative = x < 0;
        
        // Convert number to string and reverse it
        string s = to_string(abs(x));
        std::reverse(s.begin(), s.end());
        
        // Convert reversed string to an integer
        long reversedNum = stol(s);
        
        // Restore the sign
        if (isNegative) {
            reversedNum = -reversedNum;
        }
        
        // Check for overflow and return appropriately
        if (reversedNum < INT_MIN || reversedNum > INT_MAX) {
            return 0;
        }
        
        return (int)reversedNum;
    }
};
```

### Time and Space Complexity
- **Time Complexity**: O(n), where n is the number of digits in the input integer. This is due to the reversal of the string.
- **Space Complexity**: O(n), where n is the number of digits used for the string representation.

---

## Approach 2: Mathematical Approach using Modulo and Division

### Intuition
Instead of converting the integer to a string, another approach is to use a more mathematical method where we pop digits from the original number and push them into the reverse order. This involves using modulo and division operations to manipulate the digits directly and is generally more computationally efficient.

### Steps
1. Initialize `result` to 0, which will hold the reversed number.
2. Use a loop to iterate over the digits of the number until the number becomes 0:
   - Extract the last digit using the `%` operator (modulo 10).
   - Pop the last digit from the original number using integer division by 10.
   - Check for overflow/underflow conditions before appending the digit.
   - Append the digit to `result` by multiplying `result` by 10 and adding the extracted digit.
3. Return the constructed reversed number.

### Code
```cpp
class Solution {
public:
    int reverse(int x) {
        int result = 0;
        
        while (x != 0) {
            // Extract the last digit
            int digit = x % 10;
            
            // Check for overflow/underflow before appending the digit
            if (result > INT_MAX / 10 || (result == INT_MAX / 10 && digit > 7)) return 0;
            if (result < INT_MIN / 10 || (result == INT_MIN / 10 && digit < -8)) return 0;
            
            // Append the digit to the result
            result = result * 10 + digit;
            
            // Remove the last digit from the original number
            x /= 10;
        }
        
        return result;
    }
};
```

### Time and Space Complexity
- **Time Complexity**: O(n), where n is the number of digits in the input integer. This is due to iterating over each digit once.
- **Space Complexity**: O(1), as we are not using any additional data structures dependent on input size.

This mathematical approach is generally considered the optimal solution in terms of both time and space complexity.

