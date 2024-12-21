# [LeetCode 125: Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)

## Approaches
- [Approach 1: Brute Force - Use filtering and reverse operation](#approach-1)
- [Approach 2: Two Pointers Technique](#approach-2)

### Approach 1: Brute Force - Use filtering and reverse operation

**Intuition:**

To determine if a string is a valid palindrome, we need to strip out all non-alphanumeric characters, convert the string to lowercase, and then compare it with its reverse. If both strings are identical, it is a palindrome.

**Steps:**
1. Strip all non-alphanumeric characters and convert to lowercase.
2. Reverse the stripped string.
3. Compare the original filtered string with its reversed version.

**JavaScript Code:**
```javascript
var isPalindrome = function(s) {
    // Filter out non-alphanumeric characters and convert to lowercase
    let filtered = s.replace(/[^0-9a-zA-Z]/g, '').toLowerCase();
    
    // Reverse the filtered string
    let reversed = filtered.split('').reverse().join('');
    
    // Check if the filtered string is the same as its reversed version
    return filtered === reversed;
};
```

**Time Complexity:** O(n)
   - Where n is the length of the input string. Filtering and reversing are both linear operations.

**Space Complexity:** O(n)
   - An additional string is needed for the filtered result and the reversed string, both linear in size with respect to the input.

### Approach 2: Two Pointers Technique

**Intuition:**

A more optimal approach is to use two pointers starting from the beginning and end of the string. Move towards the center, skipping non-alphanumeric characters, and compare characters. This approach avoids the need to create additional strings, leading to better space efficiency.

**Steps:**
1. Initialize two pointers, one at the start (`left`) and one at the end (`right`) of the string.
2. While `left` is less than `right`, perform the following:
   - Skip any non-alphanumeric character for both pointers.
   - Compare the alphanumeric characters at these pointers.
   - If they are not equal, it's not a palindrome; return false.
   - Move `left` pointer forward and `right` pointer backward.
3. If all relevant characters matched, return true.

**JavaScript Code:**
```javascript
var isPalindrome = function(s) {
    let left = 0;
    let right = s.length - 1;
    
    while (left < right) {
        // Move left pointer to the next alphanumeric character
        while (left < right && !isAlphaNumeric(s[left])) {
            left++;
        }
        // Move right pointer to the previous alphanumeric character
        while (left < right && !isAlphaNumeric(s[right])) {
            right--;
        }
        // Compare characters case insensitively
        if (s[left].toLowerCase() !== s[right].toLowerCase()) {
            return false;
        }
        left++;
        right--;
    }
    return true;
};

// Helper function to determine if a character is alphanumeric
function isAlphaNumeric(char) {
    return (/[a-z0-9]/i).test(char);
}
```

**Time Complexity:** O(n)
   - Each character is processed at most once as we move the two pointers across the string.

**Space Complexity:** O(1)
   - This approach uses a constant amount of extra space, as we're only using a couple of pointer variables.

