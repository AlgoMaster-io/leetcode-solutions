# [Leetcode 9: Palindrome Number](https://leetcode.com/problems/palindrome-number/)

## Approaches
- [Approach 1: Convert to String and Use Two Pointers](#approach-1)
- [Approach 2: Reversing Half of the Number](#approach-2)

### Approach 1: Convert to String and Use Two Pointers

#### Intuition
A simple way to solve the problem of determining if a number is a palindrome is by treating the number like a string. This involves converting the number to a string format, then checking if the string reads the same forward and backward. We can use two pointers: one starting at the beginning of the string and the other at the end, and move them towards each other checking for equality.

#### Detailed Steps
1. Convert the number to a string.
2. Initialize two pointers, one at the beginning (`left`) and one at the end (`right`) of the string.
3. Compare the characters at these pointers. If they're not the same, it's not a palindrome.
4. Move the `left` pointer forward and the `right` pointer backward.
5. Repeat until the pointers cross each other.

#### Code

```typescript
function isPalindrome(x: number): boolean {
    // Convert the number to string
    const str = x.toString();

    // Initialize two pointers
    let left = 0;
    let right = str.length - 1;
    
    // Traverse towards the center
    while (left < right) {
        // If characters don't match, it's not a palindrome
        if (str[left] !== str[right]) {
            return false;
        }
        // Move pointers
        left++;
        right--;
    }
    return true; // All characters matched
}
```

#### Time and Space Complexity
- **Time Complexity:** O(n), where n is the number of digits in the number, as we may have to check each digit.
- **Space Complexity:** O(n), due to the storage of the number as a string.

---

### Approach 2: Reversing Half of the Number

#### Intuition
Instead of converting the number to a string, we can compare parts of the number directly. By reversing the second half of the number and comparing it with the first half, we can determine if a number is a palindrome. This avoids the need for extra space for a string representation.

#### Detailed Steps
1. Handle negative numbers immediately, as they cannot be palindromes.
2. Special case: If the last digit is 0 and the number isn't 0, it can't be a palindrome.
3. Reverse the digits of the second half of the number.
4. Compare the reversed half with the first half.
5. If they match, the number is a palindrome.

#### Code

```typescript
function isPalindrome(x: number): boolean {
    // Negative numbers are not palindromes
    // Also, numbers ending in 0 but not 0 itself can't be palindromes
    if (x < 0 || (x % 10 === 0 && x !== 0)) {
        return false;
    }
    
    let reversed = 0;
    let original = x;

    // Reverse the second half of the number
    while (x > reversed) {
        reversed = reversed * 10 + x % 10;
        x = Math.floor(x / 10);
    }
    
    // x should equal reversed, or if length is odd, ignore the middle digit
    return x === reversed || x === Math.floor(reversed / 10);
}
```

#### Time and Space Complexity
- **Time Complexity:** O(log n), where n is the number itself. We reverse half of the digits, so it's proportional to the number of digits.
- **Space Complexity:** O(1), as we only use a constant amount of space.

---

These approaches provide both a simple method using strings and a more efficient one that operates directly on the number. The second approach is preferred in terms of space efficiency.

