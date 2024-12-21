# [Leetcode 9: Palindrome Number](https://leetcode.com/problems/palindrome-number/)

## Table of Contents
- [Approach 1: Convert Number to String](#approach-1-convert-number-to-string)
- [Approach 2: Reversing Digits Approach](#approach-2-reversing-digits-approach)
- [Approach 3: Reversing Half of the Number](#approach-3-reversing-half-of-the-number)

## Approach 1: Convert Number to String

### Intuition
The simplest way to determine if a number is a palindrome is to convert it to a string and check if the string reads the same forwards and backwards.

1. Convert the integer to a string.
2. Compare the original string with its reverse. If they are the same, then the number is a palindrome.

### Code
```java
public class Solution {
    public boolean isPalindrome(int x) {
        // Convert the number to a String
        String str = Integer.toString(x);
        
        // Use two-pointer technique to compare from both ends
        int left = 0;
        int right = str.length() - 1;
        while (left < right) {
            // If characters do not match, return false
            if (str.charAt(left) != str.charAt(right)) {
                return false;
            }
            left++;
            right--;
        }
        // If all characters matched, return true
        return true;
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(n), where n is the number of digits in the number because we compare each character.
- **Space Complexity**: O(n), due to the storage used by the string representation of the number.

## Approach 2: Reversing Digits Approach

### Intuition
Instead of converting the integer to a string, we can reverse the digits of the integer itself and compare it with the original integer.

1. Handle edge cases for negative numbers and single-digit numbers.
2. Reverse the digits of the number.
3. Compare the reversed number with the original number.

### Code
```java
public class Solution {
    public boolean isPalindrome(int x) {
        // Negative numbers cannot be palindromes
        if (x < 0) {
            return false;
        }
        
        int original = x;
        int reversed = 0;
        while (x != 0) {
            // Get the last digit
            int digit = x % 10;
            
            // Add it to the reversed number
            reversed = reversed * 10 + digit;
            
            // Remove the last digit from x
            x /= 10;
        }
        
        // Compare the reversed number with the original number
        return original == reversed;
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(n), where n is the number of digits in the number.
- **Space Complexity**: O(1), since we're only using a few integers for temporary storage.

## Approach 3: Reversing Half of the Number

### Intuition
We can improve upon the previous approach by only reversing half of the number and checking if the first half equals the reversed second half. This reduces the amount of work needed, particularly for very long numbers.

1. A number is a palindrome if dividing and reversing its first half equals the second half.
2. Reverse the second half of the number and compare it with the first half.

### Code
```java
public class Solution {
    public boolean isPalindrome(int x) {
        // Negative numbers and numbers ending with 0 are not palindromes (except 0 itself)
        if (x < 0 || (x % 10 == 0 && x != 0)) {
            return false;
        }
        
        int reversedHalf = 0;
        while (x > reversedHalf) {
            // Extract the last digit and build the reversed half
            int digit = x % 10;
            reversedHalf = reversedHalf * 10 + digit;
            x /= 10;
        }
        
        // If the number is odd, reversedHalf will have an extra digit
        // Which we need to remove by integer division
        return x == reversedHalf || x == reversedHalf / 10;
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(log n), because we're processing roughly half of the number's digits.
- **Space Complexity**: O(1), as we only use a few integer variables for temporary storage.

