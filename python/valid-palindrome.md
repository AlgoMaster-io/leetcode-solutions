# [Leetcode 125: Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)

## Approaches
1. [Two-Pointer Approach with String Builder](#approach-1)
2. [Optimized Two-Pointer Approach (Without Extra Space)](#approach-2)

## Approach 1: Two-Pointer Approach with String Builder

### Intuition

The essence of this problem is to check if a given string, after cleaning up non-alphanumeric characters, is a palindrome. A palindrome reads the same forward and backward. The simplest way to solve this is by using two pointers. We can optimize this approach by building a cleaned version of the string and then use two pointers to compare characters from both ends of this cleaned string.

### Algorithm
1. Initialize an empty string or list to store alphanumeric characters in lowercase.
2. Traverse the given string:
   - For each character, check if it is alphanumeric.
   - If it is, convert it to lowercase and append to the list.
3. Initialize two pointers: one at the start (`left`) and one at the end (`right`) of the list.
4. Loop while `left` is less than `right`:
   - Check if the characters at `left` and `right` are the same.
   - If not, return `False` because it's not a palindrome.
   - Otherwise, move the `left` pointer to the right and `right` to the left.
5. If loop exits without returning `False`, return `True`.

### Code
```python
def isPalindrome(s: str) -> bool:
    # Create a list to hold only alphanumeric characters in lowercase form
    filtered_chars = [char.lower() for char in s if char.isalnum()]
    
    # Initialize two pointers
    left, right = 0, len(filtered_chars) - 1
    
    # Use two pointers to check palindrome
    while left < right:
        # Compare characters and move pointers
        if filtered_chars[left] != filtered_chars[right]:
            return False
        left, right = left + 1, right - 1
    
    return True
```

### Time Complexity
- O(n): We traverse the string once to filter characters and once again with two pointers.
  
### Space Complexity
- O(n): In the worst case, all characters are stored in the filtered list.

## Approach 2: Optimized Two-Pointer Approach (Without Extra Space)

### Intuition

The previous approach uses extra memory to construct a filtered version of the string. We can optimize this by managing the pointers directly on the original string, skipping non-alphanumeric characters and comparing characters in lower case as we go.

### Algorithm
1. Initialize two pointers: `left` at the beginning and `right` at the end of the string.
2. Loop while `left` is less than `right`:
   - Move the `left` pointer forward if the current character is not alphanumeric.
   - Move the `right` pointer backward if the current character is not alphanumeric.
   - If both characters are alphanumeric, check if they are equal disregarding case.
   - If they are not equal, return `False`.
   - If they are equal, move both pointers inward.
3. If the loop exits, return `True`.

### Code
```python
def isPalindrome(s: str) -> bool:
    # Initialize two pointers
    left, right = 0, len(s) - 1
    
    while left < right:
        # Move left pointer if not alphanumeric
        while left < right and not s[left].isalnum():
            left += 1
        # Move right pointer if not alphanumeric
        while left < right and not s[right].isalnum():
            right -= 1
        
        # Compare characters in a case-insensitive manner
        if s[left].lower() != s[right].lower():
            return False
        
        # Move both pointers towards the center
        left += 1
        right -= 1
        
    return True
```

### Time Complexity
- O(n): Each character is processed at most twice.

### Space Complexity
- O(1): No additional space is used besides a few variables.

