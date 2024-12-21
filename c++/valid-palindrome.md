# [Leetcode 125: Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)

## Approaches
1. [Two-Pointer Approach](#1-two-pointer-approach)

---

### 1. Two-Pointer Approach

**Intuition:**

A palindrome reads the same backward as forward, ignoring non-alphanumeric characters and case sensitivity. The two-pointer approach is efficient for such problems as it allows us to approach the problem from both ends towards the center, checking character equality.

1. **Step-by-step Explanation:**
   - Initialize two pointers: `left` starting at the beginning of the string, and `right` starting at the end of the string.
   - Move the `left` pointer rightwards to skip non-alphanumeric characters.
   - Move the `right` pointer leftwards to skip non-alphanumeric characters.
   - Compare the characters at the `left` and `right` pointers only if they are valid (i.e., alphanumeric).
   - If the characters are not equal (ignoring case), the string is not a palindrome.
   - If they are equal, increment the `left` pointer and decrement the `right` pointer.
   - Continue until the `left` pointer is greater than or equal to the `right` pointer, confirming that the string is a palindrome.

```cpp
#include <iostream>
#include <cctype> // for isalnum, tolower

class Solution {
public:
    bool isPalindrome(std::string s) {
        int left = 0, right = s.size() - 1;
        
        while (left < right) {
            // Move left pointer to the next valid alphanumeric character
            while (left < right && !isalnum(s[left])) {
                left++;
            }
            // Move right pointer to the previous valid alphanumeric character
            while (left < right && !isalnum(s[right])) {
                right--;
            }
            
            // Compare the characters in lowercase
            if (tolower(s[left]) != tolower(s[right])) {
                return false;
            }
            left++;
            right--;
        }
        
        return true; // If all matched
    }
};
```

**Time Complexity:** O(n), where n is the length of the string. We examine each character at most once.

**Space Complexity:** O(1), as no additional space proportional to the input size is used, aside from the input itself.

