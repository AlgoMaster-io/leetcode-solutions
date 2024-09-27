# Minimum Window Substring
Given two strings s and t of lengths m and n respectively, return the minimum window substring of s such that every character in t (including duplicates) is included in the window. If there is no such substring, return the empty string "".

### Constraints:
- 1 <= s.length, t.length <= 10^5
- s and t consist of uppercase and lowercase English letters.

### Examples
```javascript
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
Explanation: The minimum window substring is "BANC".

Input: s = "a", t = "a"
Output: "a"
Explanation: The entire string `s` is the minimum window.

Input: s = "a", t = "aa"
Output: ""
Explanation: Since `t` contains two 'a' characters but the string `s` contains only one, it is impossible to form the desired substring.
```

### Follow-up:
Could you find an algorithm that runs in O(m + n) time?

## Approaches to Solve the Problem
### Approach 1: Brute Force (Inefficient)
##### Intuition:
A brute-force solution would be to generate all possible substrings of s and check if they contain all characters of t. This approach is inefficient for large strings due to the huge number of substrings.

Steps:
1. Loop over all possible substrings of s.
2. For each substring, check if it contains all the characters of t by using a frequency map.
3. Track the minimum window that contains all characters of t.
##### Time Complexity:
O(m² * n), where m is the length of s and n is the length of t. We check every substring of s and compare it with t.
##### Space Complexity:
O(m + n), for the substring and character frequency maps.
##### Python Code:
```python
from collections import Counter

def minWindow(s: str, t: str) -> str:
    if not s or not t:
        return ""
    
    min_len = float('inf')
    min_window = ""
    
    for i in range(len(s)):
        for j in range(i + 1, len(s) + 1):
            substring = s[i:j]
            if all(substring.count(char) >= t.count(char) for char in t):
                if len(substring) < min_len:
                    min_len = len(substring)
                    min_window = substring
    
    return min_window
```
### Approach 2: Sliding Window with Two Pointers (Optimal Solution)
##### Intuition: 
The optimal solution involves using the sliding window technique with two pointers, left and right, along with character frequency maps. The idea is to expand the window by moving right to include characters until the window contains all characters from t. Then, we shrink the window by moving left to try and minimize the window size while still keeping all required characters.

Steps:
1. Create a frequency map for t to know how many of each character is needed.
2. Use two pointers (left and right) to represent the window in s.
3. Expand the window by moving right until all characters in t are included in the window.
4. Once the window contains all required characters, shrink the window by moving left to minimize its size.
5. Track the smallest valid window encountered.
##### Visualization:
```perl
For s = "ADOBECODEBANC" and t = "ABC":

Initial state: left = 0, right = 0, valid = False
Step 1: Expand window → right = 5, window = "ADOBEC" (valid window)
Step 2: Shrink window → left = 3, window = "BEC" (valid and smaller window)
Continue expanding and shrinking until the minimum valid window is found.
```
##### Time Complexity:
O(m + n), where m is the length of s and n is the length of t. We scan the string s once and use a frequency map to manage the window.
##### Space Complexity:
O(n), where n is the number of unique characters in t, because we use a frequency map.
##### Python Code:
```python
from collections import Counter

def minWindow(s: str, t: str) -> str:
    if not s or not t:
        return ""
    
    # Step 1: Create a frequency map for t
    t_count = Counter(t)
    required = len(t_count)  # Number of unique characters in t that need to be in the window
    
    # Step 2: Initialize the sliding window variables
    left, right = 0, 0
    window_count = {}
    formed = 0  # Number of unique characters in the current window that match the frequency in t
    min_len = float('inf')
    min_window = (0, 0)
    
    # Step 3: Expand the window by moving the right pointer
    while right < len(s):
        char = s[right]
        window_count[char] = window_count.get(char, 0) + 1
        
        # Check if the current character matches the frequency in t
        if char in t_count and window_count[char] == t_count[char]:
            formed += 1
        
        # Step 4: Try to shrink the window by moving the left pointer
        while left <= right and formed == required:
            char = s[left]
            
            # Update the minimum window
            if right - left + 1 < min_len:
                min_len = right - left + 1
                min_window = (left, right)
            
            # Shrink the window
            window_count[char] -= 1
            if char in t_count and window_count[char] < t_count[char]:
                formed -= 1
            
            left += 1
        
        right += 1
    
    # Step 5: Return the result
    l, r = min_window
    return s[l:r+1] if min_len != float('inf') else ""
```
### Approach 3: Optimized Sliding Window with Array Frequency Map
##### Intuition: 
For better performance in terms of space, we can use a fixed-size array instead of a hash map to track the frequency of characters. This approach works because we only deal with lowercase and uppercase English letters, which limits the possible characters to 52 (A-Z and a-z). By using an array of size 128 (to handle all ASCII characters), we can directly map characters to their indices.

Steps:
1. Use an array of size 128 to store the frequency of characters in t.
2. Use the same array to track the characters in the current window of s.
3. Slide the window using two pointers and compare the frequency of characters in the window with t.
##### Time Complexity:
O(m + n), where m is the length of s and n is the length of t.
##### Space Complexity:
O(1), since we use a fixed array of size 128 for character frequencies.
##### Python Code:
```python
def minWindow(s: str, t: str) -> str:
    if not s or not t:
        return ""
    
    # Step 1: Create an array for the frequency of characters in t
    t_count = [0] * 128
    for char in t:
        t_count[ord(char)] += 1
    
    # Step 2: Initialize sliding window variables
    left, right, start = 0, 0, 0
    min_len = float('inf')
    required = len(t)
    
    # Step 3: Expand the window
    while right < len(s):
        if t_count[ord(s[right])] > 0:
            required -= 1
        
        t_count[ord(s[right])] -= 1
        right += 1
        
        # Step 4: Shrink the window
        while required == 0:
            if right - left < min_len:
                min_len = right - left
                start = left
            
            t_count[ord(s[left])] += 1
            if t_count[ord(s[left])] > 0:
                required += 1
            
            left += 1
    
    return s[start:start + min_len] if min_len != float('inf') else ""
```
## Summary
| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Brute Force	                    | 	O(m² * n)      | O(m + n)             |
| Sliding Window with Two Pointers		        | O(m + n)	            | O(n)             |
| Optimized Sliding Window with Array		              | O(m + n)            | O(1)             |

The Sliding Window with Two Pointers is the most optimal and practical solution. The array-based frequency map can be a further optimization in terms of space for problems involving ASCII characters.