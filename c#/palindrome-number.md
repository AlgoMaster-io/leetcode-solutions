## [Leetcode 9: Palindrome Number](https://leetcode.com/problems/palindrome-number/)

### Approaches
- [Approach 1: Convert to String](#approach-1-convert-to-string)
- [Approach 2: Reverse Half of the Number](#approach-2-reverse-half-of-the-number)

### Approach 1: Convert to String

**Intuition:**

The simplest way to check if a number is a palindrome is to convert it to a string and then check if the string reads the same forward and backward. This method leverages the inherent reversal capabilities of string operations in C#.

**Steps:**
1. Convert the integer to a string.
2. Compare the original string to its reverse.
3. If they are the same, the number is a palindrome.

**Code:**

```csharp
public class Solution {
    public bool IsPalindrome(int x) {
        // Negative numbers are not palindromes
        if (x < 0) return false;
        
        // Convert the number to a string
        string s = x.ToString();
        
        // Reverse the string and compare
        char[] charArray = s.ToCharArray();
        Array.Reverse(charArray);
        string reversed = new string(charArray);
        
        // Check if the original string and the reversed string are equal
        return s.Equals(reversed);
    }
}
```

**Time Complexity:** O(n), where n is the number of digits in the number.
**Space Complexity:** O(n), as we store the string and its reverse.

### Approach 2: Reverse Half of the Number

**Intuition:**

Instead of converting the number to a string, we can reverse only the second half of the number and compare it with the first half. Since reversing a number directly without converting to another format is tricky, reversing only half avoids overflow issues.

**Steps:**
1. Handle special cases where x is negative or ends with 0 but is not 0.
2. Initialize variables to hold the reversed half.
3. Loop through the number, reversing the digits until the original number becomes less than or equal to the reversed half.
4. Compare the two halves to determine if the number is a palindrome.

**Code:**

```csharp
public class Solution {
    public bool IsPalindrome(int x) {
        // Special cases:
        // When x is negative or if the last digit is 0 (and x is not 0), it cannot be a palindrome
        if (x < 0 || (x % 10 == 0 && x != 0)) {
            return false;
        }

        int reversedHalf = 0;
        while (x > reversedHalf) {
            // Move the last digit from x to reversedHalf
            reversedHalf = reversedHalf * 10 + x % 10;
            // Remove the last digit from x
            x /= 10;
        }
        
        // The number is a palindrome if the original half is equal to the reversed half
        // In case of odd number of digits, we can remove the middle digit by reversedHalf / 10
        return x == reversedHalf || x == reversedHalf / 10;
    }
}
```

**Time Complexity:** O(log10(n)), where n is the integer and the loop runs only half of the number's digits.
**Space Complexity:** O(1), as we're using a constant amount of space.

