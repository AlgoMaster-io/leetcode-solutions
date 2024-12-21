# [Leetcode 125: Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)

## Approaches:
- [Approach 1: Two-pointer Approach](#approach-1-two-pointer-approach)

---

### Approach 1: Two-pointer Approach

#### Intuition:
A palindrome is a string that reads the same forwards and backwards, ignoring non-alphanumeric characters and case. To check for a valid palindrome, we can use a two-pointer technique where one pointer starts at the beginning of the string and the other starts at the end. We compare the characters and move the pointers towards the center, skipping any non-alphanumeric characters.

- Begin with two pointers, `left` at the start of the string and `right` at the end.
- Move the `left` pointer to the right until it points to an alphanumeric character.
- Move the `right` pointer to the left until it points to an alphanumeric character.
- Compare characters at `left` and `right`, if they are different then it's not a palindrome.
- Repeat until the `left` pointer meets or surpasses the `right` pointer.
- Normalize alphanumeric character comparison by converting both to lowercase.

#### Code:
```csharp
public class Solution {
    public bool IsPalindrome(string s) {
        // Initialize the two pointers
        int left = 0;
        int right = s.Length - 1;
        
        while (left < right) {
            // Move the left pointer to the right as long as it's not at an alphanumeric character
            while (left < right && !Char.IsLetterOrDigit(s[left])) {
                left++;
            }
            
            // Move the right pointer to the left as long as it's not at an alphanumeric character
            while (left < right && !Char.IsLetterOrDigit(s[right])) {
                right--;
            }
            
            // Compare the characters at the two pointers, ignoring case
            if (Char.ToLower(s[left]) != Char.ToLower(s[right])) {
                return false; // Characters do not match
            }
            
            // Move both pointers towards the center
            left++;
            right--;
        }
        
        return true; // The string is a palindrome
    }
}
```

#### Time Complexity:
- **O(n)**: We traverse the string only once, checking and comparing each character pair at most once.

#### Space Complexity:
- **O(1)**: The solution uses a constant amount of additional space.


