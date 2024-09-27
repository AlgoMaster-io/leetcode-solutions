
# Valid Palindrome
Given a string s, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

### Constraints:
- 1 <= s.length <= 2 * 10^5
- s consists only of printable ASCII characters.

### Examples
```javascript
Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.

Input: s = "race a car"
Output: false
Explanation: "raceacar" is not a palindrome.
```

## Approaches to Solve the Problem
### Approach 1: Clean the String and Check for Palindrome
##### Intuition:
The simplest approach is to clean the string by removing all non-alphanumeric characters and converting all letters to lowercase. Once the string is cleaned, checking for a palindrome is straightforward: reverse the string and compare it with the original.

Steps:
1. Traverse the string s and remove all non-alphanumeric characters.
2. Convert all remaining characters to lowercase.
3. Check if the cleaned string is equal to its reverse.
##### Time Complexity:
O(n), where n is the length of the string. We traverse the string once to clean it and once more to compare it with its reverse.
##### Space Complexity:
O(n), as we store the cleaned version of the string.
##### Python Code:
```python
def isPalindrome(s: str) -> bool:
    # Clean the string by keeping only alphanumeric characters and converting to lowercase
    cleaned = ''.join(char.lower() for char in s if char.isalnum())
    # Check if the cleaned string is the same as its reverse
    return cleaned == cleaned[::-1]
```
### Approach 2: Two Pointers (Most Optimal Solution)
##### Intuition: 
A more efficient approach is to avoid creating a new string altogether. We can use two pointers, one starting from the beginning and the other from the end, and move towards the center while ignoring non-alphanumeric characters. Compare the characters directly, converting them to lowercase if necessary.

Steps:
1. Initialize two pointers: left at the start of the string and right at the end.
2. Move both pointers toward the center while skipping non-alphanumeric characters.
3. For each valid character pair, check if they are the same (ignoring case).
4. If any mismatch occurs, return False.
5. If the pointers cross without any mismatch, return True.

##### Visualization:
```perl
For the string "A man, a plan, a canal: Panama":

The two pointers approach compares 'A' and 'a', 'm' and 'm', 'a' and 'a', ignoring spaces, commas, and colons until they meet in the middle.
```
##### Time Complexity:
O(n), where n is the length of the string. Each character is checked at most once.
##### Space Complexity:
O(1), as no extra space is required beyond a few variables for the pointers.
##### Python Code:
```python
def isPalindrome(s: str) -> bool:
    left, right = 0, len(s) - 1
    
    while left < right:
        # Move left pointer until we find an alphanumeric character
        while left < right and not s[left].isalnum():
            left += 1
        # Move right pointer until we find an alphanumeric character
        while left < right and not s[right].isalnum():
            right -= 1
        
        # Compare the characters at left and right pointers, ignoring case
        if s[left].lower() != s[right].lower():
            return False
        
        left += 1
        right -= 1
    
    return True
```
## Summary

| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Clean the String and Check Palindrome	                    | O(n)      | O(n)             |
| Two Pointers	                          | O(n)            | O(n)             |

The Two Pointers approach is the most optimal solution as it avoids extra space usage and checks for a palindrome in a single pass using constant space.