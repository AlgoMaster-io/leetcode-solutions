# [Leetcode 9: Palindrome Number](https://leetcode.com/problems/palindrome-number/)

## Approaches
1. [String Conversion Approach](#string-conversion-approach)
2. [Reversing Half the Number](#reversing-half-the-number)

### String Conversion Approach

This approach involves converting the number to a string and checking if the string reads the same forwards and backwards.

#### Intuition:
- We first convert the integer to a string.
- We then check if the string is equal to its reverse.
- If it is, the number is a palindrome.

#### Code:

```go
func isPalindrome(x int) bool {
    // Negative numbers are not palindromes
    if x < 0 {
        return false
    }
    
    // Convert the integer to a string
    str := fmt.Sprint(x)
    
    // Initialize two pointers, one at the start and one at the end of the string
    left, right := 0, len(str) - 1
    
    // Compare characters while the pointers haven't crossed
    for left < right {
        // If characters at the pointers don't match, it's not a palindrome
        if str[left] != str[right] {
            return false
        }
        // Move pointers towards the center
        left++
        right--
    }
    
    // If all characters matched, it's a palindrome
    return true
}
```

#### Time Complexity:
- O(n), where n is the number of digits in the number, due to checking each character in the string representation.

#### Space Complexity:
- O(n) due to the extra space required to store the string representation of x.

---

### Reversing Half the Number

This approach involves reversing half of the number and comparing it to the other half.

#### Intuition:
- A negative number cannot be a palindrome because it starts with a negative sign.
- If the last digit is 0, the first digit must also be 0 for it to be a palindrome (only possible if the number is 0).
- Compare the original number with the reversed last half. Reverse half of the number and compare with the first half.

#### Code:

```go
func isPalindrome(x int) bool {
    // Special case: negative numbers or numbers ending in 0 but not 0 itself cannot be palindromes
    if x < 0 || (x % 10 == 0 && x != 0) {
        return false
    }
    
    reversed := 0
    number := x
    
    // Reverse half of the number
    for number > reversed {
        // Add the last digit of the number to the reversed number
        reversed = reversed * 10 + number % 10
        // Remove the last digit from the number
        number /= 10
    }
    
    // Check if the number matches the reversed version (for even number of digits) or if the reversed 
    // number divided by 10 matches the rest of the number (for odd number of digits)
    return number == reversed || number == reversed / 10
}
```

#### Time Complexity:
- O(log10(x)), where x is the input number. This is because we are dividing the number by 10 in each iteration.

#### Space Complexity:
- O(1), as we are not using additional space proportional to the input size.

Each of these approaches uses different methods to verify the palindrome property, and the optimal approach should be chosen based on the constraints and requirements of the problem.

