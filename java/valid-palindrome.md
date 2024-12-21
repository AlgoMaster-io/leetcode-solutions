# [Leetcode 125: Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)

## Approaches
- [Approach 1: Two-pointer with StringBuilder](#approach-1)
- [Approach 2: Two-pointer optimized for space](#approach-2)

---

## Approach 1: Two-pointer with StringBuilder

### Intuition:

The problem requires us to determine if a given string is a palindrome, considering only alphanumeric characters and ignoring cases. The basic idea is to first clean the string by removing all non-alphanumeric characters and converting everything to a consistent case (lowercase). Then, use two pointers to compare characters from the start and end of the string moving inwards.

### Steps:

1. Use a `StringBuilder` to create a cleaned version of the string that contains only lowercase alphanumeric characters.
2. Initialize two pointers, one at the start and another at the end of the cleaned string.
3. Increment and decrement the pointers towards the center, checking if the characters are the same.
4. If you reach the center with all characters matching, the string is a palindrome.

### Java Code:

```java
public boolean isPalindrome(String s) {
    // Step 1: Clean the string using StringBuilder
    StringBuilder cleaned = new StringBuilder();
    
    for (char c : s.toCharArray()) {
        if (Character.isLetterOrDigit(c)) {
            cleaned.append(Character.toLowerCase(c));
        }
    }
    
    // Step 2: Initialize two pointers
    int left = 0;
    int right = cleaned.length() - 1;
    
    // Step 3: Check palindrome property
    while (left < right) {
        if (cleaned.charAt(left) != cleaned.charAt(right)) {
            return false; // Not a palindrome if any mismatch occurs
        }
        left++;
        right--;
    }
    
    return true; // String is a palindrome
}
```

### Complexity Analysis:

- **Time Complexity:** O(n), where n is the length of the input string. We traverse each character twice in the worst case (once for cleaning and once for palindrome checking).
- **Space Complexity:** O(n), used for the cleaned string stored in the `StringBuilder`.

---

## Approach 2: Two-pointer optimized for space

### Intuition:

The previous solution effectively checks the palindrome by cleaning up the string first. However, we can optimize it to not require extra space for a cleaned version. Instead, directly use two pointers on the input string to perform the checking while skipping over non-alphanumeric characters.

### Steps:

1. Initialize two pointers, left at the start and right at the end of the string.
2. While left is less than right:
   - Increment the left pointer until an alphanumeric character is found.
   - Decrement the right pointer until an alphanumeric character is found.
   - Compare the characters. If they are not equal, the string is not a palindrome.
   - Move the pointers inward and repeat.
3. If the entire string is processed without mismatches, it is a palindrome.

### Java Code:

```java
public boolean isPalindrome(String s) {
    // Step 1: Initialize two pointers
    int left = 0;
    int right = s.length() - 1;
    
    // Step 2: Check palindrome property with skipping non-alphanumeric
    while (left < right) {
        // Find the next valid alphanumeric character from left
        while (left < right && !Character.isLetterOrDigit(s.charAt(left))) {
            left++;
        }
        
        // Find the next valid alphanumeric character from right
        while (left < right && !Character.isLetterOrDigit(s.charAt(right))) {
            right--;
        }
        
        // Compare characters in a case-insensitive manner
        if (Character.toLowerCase(s.charAt(left)) != Character.toLowerCase(s.charAt(right))) {
            return false; // Not a palindrome if any mismatch occurs
        }
        
        // Move pointers inward
        left++;
        right--;
    }
    
    return true; // String is a palindrome
}
```

### Complexity Analysis:

- **Time Complexity:** O(n), where n is the length of the input string. We traverse each character at most twice.
- **Space Complexity:** O(1), no additional space is used beyond integer indices.

