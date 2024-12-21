[Leetcode Problem 9: Palindrome Number](https://leetcode.com/problems/palindrome-number/)

## Approaches
1. [Convert Integer to String and Check](#approach-1)
2. [Reversing an Integer](#approach-2)
3. [Reversed Half Compare](#approach-3)

### Approach 1: Convert Integer to String and Check
#### Intuition:
The most straightforward approach is to convert the integer to a string and check if it reads the same forwards and backwards. This is a direct application of the definition of a palindrome.

- Convert the integer to a string.
- Compare the string with its reverse.
- The integer is a palindrome if and only if the string matches its reversed version.

```cpp
bool isPalindrome(int x) {
    // Negative numbers are not palindromes
    if (x < 0) return false;
    
    // Convert integer to string
    std::string str = std::to_string(x);
    
    // Initialize reverse of the string
    std::string revStr = str;
    std::reverse(revStr.begin(), revStr.end());
    
    // Check if the original string is equal to its reverse
    return str == revStr;
}
```

- **Time Complexity**: \(O(n)\), where \(n\) is the number of digits since reversing a string takes linear time.
- **Space Complexity**: \(O(n)\), due to the storage of the string.

### Approach 2: Reversing an Integer
#### Intuition:
Instead of using strings, another approach involves reversing the digits of the number and checking if the reversed number matches the original number. 

- Handle special cases for negative numbers and numbers ending in zero.
- Reverse the number digit by digit until the original number is zero.
- Compare the reversed number to the original number.

```cpp
bool isPalindrome(int x) {
    // Negative numbers and numbers ending in zero (except 0 itself) are not palindromes
    if (x < 0 || (x != 0 && x % 10 == 0)) return false;

    int original = x;
    long long reversed = 0;
    
    // Reverse the number
    while (x != 0) {
        int digit = x % 10;  // Get the last digit
        reversed = reversed * 10 + digit;
        x /= 10;  // Remove the last digit
    }
    
    // Check if the original number is equal to the reversed number
    return original == reversed;
}
```

- **Time Complexity**: \(O(n)\), where \(n\) is the number of digits, since each digit is processed once.
- **Space Complexity**: \(O(1)\), as we're utilizing primitive variables only.

### Approach 3: Reversed Half Compare
#### Intuition:
To avoid potential overflow when reversing the entire number, reverse only half of the digits and compare it with the other half.

- If the length of the number's digits is odd, discard the middle digit after reversing the second half.
- This approach optimizes performance and avoids overflow issues.

```cpp
bool isPalindrome(int x) {
    // Negative numbers and numbers ending in zero (except 0 itself) are not palindromes
    if (x < 0 || (x != 0 && x % 10 == 0)) return false;

    int reversedHalf = 0;
    
    // Reverse half of the number
    while (x > reversedHalf) {
        int digit = x % 10;  // Get the last digit
        reversedHalf = reversedHalf * 10 + digit;
        x /= 10;  // Remove the last digit
    }
    
    // Check for even and odd digit numbers
    return x == reversedHalf || x == reversedHalf / 10;
}
```

- **Time Complexity**: \(O(n/2) = O(n)\), (where \(n\) is the number of digits) since we're processing half of the digits.
- **Space Complexity**: \(O(1)\), as we only use a few integer variables.

