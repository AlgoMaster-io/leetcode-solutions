# Longest Substring Without Repeating Characters
Given a string s, find the length of the longest substring without repeating characters.

### Constraints:
- 0 <= s.length <= 5 * 10^4
- s consists of English letters, digits, symbols, and spaces.

### Examples
```javascript
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.

Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.

Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3. 
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

## Approaches to Solve the Problem
### Approach 1: Brute Force (Inefficient)
##### Intuition:
A simple brute-force solution is to check every possible substring, and for each substring, check if it contains duplicate characters. The maximum length of a substring without repeating characters is tracked.

Steps:
1. Use two nested loops to generate all possible substrings.
2. For each substring, check if it contains duplicate characters by using a set or dictionary.
3. Track the maximum length of substrings without duplicates.
##### Time Complexity:
O(n²) for checking all substrings and validating each one.
##### Space Complexity:
O(n) for storing characters in a set or dictionary while checking each substring.
##### Python Code:
```python
def lengthOfLongestSubstring(s: str) -> int:
    max_len = 0
    n = len(s)
    
    for i in range(n):
        seen = set()
        for j in range(i, n):
            if s[j] in seen:
                break
            seen.add(s[j])
            max_len = max(max_len, j - i + 1)
    
    return max_len
```
### Approach 2: Sliding Window with Hash Set (Optimal)
##### Intuition: 
The brute-force approach is inefficient because it recalculates the substring from scratch each time. We can improve this by using a sliding window technique, where we keep track of the current substring using two pointers, left and right, and adjust the window as necessary. A hash set is used to track characters in the current window, allowing us to efficiently check for duplicates.

1. Use two pointers (left and right) to create a window of characters.
2. Expand the window by moving the right pointer.
3. If a duplicate character is found, shrink the window from the left by moving the left pointer until the duplicate is removed.
4. Track the maximum length of the window during this process.

Steps:
1. Initialize a set to store the characters in the current window.
2. Move the right pointer to expand the window.
3. If the character at right is already in the set, move the left pointer to shrink the window until the character is removed.
4. Keep updating the maximum length of the window.
##### Visualization:
```perl
For s = "abcabcbb":

Initial state: left = 0, right = 0, window = ""
Step 1: Expand window to "a"
Step 2: Expand window to "ab"
Step 3: Expand window to "abc" (max_len = 3)
Step 4: Encounter duplicate 'a', move left to 1 (window = "bca")
Step 5: Expand window to "bcab" → encounter duplicate 'b', move left to 2 (window = "cab")
Continue until the entire string is processed.
```
##### Time Complexity:
O(n), where n is the length of the string. We traverse the string once with the sliding window.
##### Space Complexity:
O(min(n, m)), where n is the length of the string and m is the size of the character set (e.g., 26 for lowercase English letters).
##### Python Code:
```python
def lengthOfLongestSubstring(s: str) -> int:
    char_set = set()
    left = 0
    max_len = 0
    
    for right in range(len(s)):
        # Shrink the window if we encounter a duplicate character
        while s[right] in char_set:
            char_set.remove(s[left])
            left += 1
        
        # Add the current character to the window
        char_set.add(s[right])
        
        # Update the maximum length
        max_len = max(max_len, right - left + 1)
    
    return max_len
```
### Approach 3: Sliding Window with Hash Map
##### Intuition: 
A further optimization can be made by using a hash map to store the last index of each character. This allows us to move the left pointer directly to the position after the last occurrence of the duplicate character, rather than shrinking the window one character at a time.

Steps:
1. Initialize a hash map to store the last index of each character.
2. Move the right pointer to expand the window.
3. If a duplicate character is found, move the left pointer directly to the position after the last occurrence of the duplicate.
4. Track the maximum length of the window.
##### Visualization:
```perl
For s = "abcabcbb":

Initial state: left = 0, right = 0, window = "", last_occurrence = {}
Step 1: Expand window to "a" → last_occurrence['a'] = 0
Step 2: Expand window to "ab" → last_occurrence['b'] = 1
Step 3: Expand window to "abc" → last_occurrence['c'] = 2
Step 4: Encounter duplicate 'a', move left to 1 (after last 'a')
Continue until the entire string is processed.
```
##### Time Complexity:
O(n), as we still traverse the string once, but this time we move the left pointer more efficiently.
##### Space Complexity:
O(min(n, m)), where n is the length of the string and m is the size of the character set.
##### Python Code:
```python
def lengthOfLongestSubstring(s: str) -> int:
    char_index_map = {}
    left = 0
    max_len = 0
    
    for right in range(len(s)):
        # If the character is already in the map, move the left pointer
        if s[right] in char_index_map:
            left = max(left, char_index_map[s[right]] + 1)
        
        # Update the last occurrence of the current character
        char_index_map[s[right]] = right
        
        # Update the maximum length of the window
        max_len = max(max_len, right - left + 1)
    
    return max_len
```
## Summary
| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Brute Force	                    | 	O(n²)      | O(n)             |
| Sliding Window with Hash Set		                   | O(n)            | O(min(n, m))             |
| Sliding Window with Hash Map		                   | O(n)            | O(min(n, m))             |

The Sliding Window with Hash Map is the most efficient approach because it uses a single pass over the string and minimizes the movement of the left pointer by jumping directly to the correct position.