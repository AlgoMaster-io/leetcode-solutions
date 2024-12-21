# [Leetcode 125: Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)

## Approaches:

1. [Two-Pointer Approach](#two-pointer-approach)
2. [Optimized with Regular Expressions](#optimized-with-regular-expressions)

### Two-Pointer Approach

**Intuition**:  
A palindrome reads the same backward as forward. The challenge is to ignore characters that are not alphanumeric while implementing the check. A two-pointer approach is well-suited here: one pointer starts from the beginning and another from the end, moving towards each other, to check if characters are the same, ignoring non-alphanumeric characters.

**Steps**:
1. Initialize two pointers, `left` at the beginning (0) and `right` at the end (s.length - 1) of the string.
2. While `left` is less than `right`:
   - Move the `left` pointer to the right until an alphanumeric character is found.
   - Move the `right` pointer to the left until an alphanumeric character is found.
   - Compare characters at `left` and `right`, converting characters to the same case for a fair comparison.
   - If the characters differ, the string is not a palindrome.
   - Move both pointers closer towards the center.
3. If no mismatches occur, the string is a valid palindrome.

**Code**:

```typescript
function isValidPalindrome(s: string): boolean {
    // Helper function to check if a character is alphanumeric
    const isAlphaNumeric = (char: string): boolean => {
        return /^[a-zA-Z0-9]$/.test(char);
    };
    
    let left = 0;
    let right = s.length - 1;
    
    while (left < right) {
        // Move left pointer right until it points to a valid alphanumeric character
        while (left < right && !isAlphaNumeric(s[left])) {
            left++;
        }
        // Move right pointer left until it points to a valid alphanumeric character
        while (left < right && !isAlphaNumeric(s[right])) {
            right--;
        }
        
        // Compare the characters, ignoring cases by converting to lowercase
        if (s[left].toLowerCase() !== s[right].toLowerCase()) {
            return false;
        }
        
        // Move both pointers towards the center
        left++;
        right--;
    }
    
    return true;
}
```

**Time Complexity**: O(n), where n is the length of the string. Each character is checked at most once.
**Space Complexity**: O(1), as no additional space is used that scales with input size.

---

### Optimized with Regular Expressions

**Intuition**:  
By using a regular expression, we can efficiently filter out non-alphanumeric characters, which simplifies the palindrome check to a straightforward comparison of processed characters. This approach reduces the overhead of handling each character's validity during the comparison loop.

**Steps**:
1. Use a regular expression to filter the input string, keeping only lowercase alphanumeric characters.
2. Compare the processed string with its reverse.
3. If they are the same, it is a valid palindrome.

**Code**:

```typescript
function isValidPalindromeOptimized(s: string): boolean {
    // Use regex to filter only alphanumeric characters, convert to lowercase
    const filtered = s.replace(/[^a-z0-9]/gi, '').toLowerCase();
    // Compare the filtered string with its reverse
    const reversed = filtered.split('').reverse().join('');
    return filtered === reversed;
}
```

**Time Complexity**: O(n), where n is the length of the string. Regular expression operations and string reverse are linear time.
**Space Complexity**: O(n), since we store the filtered string and its reverse.

