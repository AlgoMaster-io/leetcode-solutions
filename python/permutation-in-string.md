# Permutation in String
Given two strings s1 and s2, return true if s2 contains a permutation of s1, or false otherwise. In other words, return true if one of s1's permutations is the substring of s2.

### Constraints:
- 1 <= s1.length, s2.length <= 10^4
- s1 and s2 consist of lowercase English letters.

### Examples
```javascript
Input: s1 = "ab", s2 = "eidbaooo"
Output: true
Explanation: s2 contains one permutation of s1 ("ba").

Input: s1 = "ab", s2 = "eidboaoo"
Output: false
```

## Approaches to Solve the Problem
### Approach 1: Brute Force (Inefficient)
##### Intuition:
The brute-force approach would involve generating all possible permutations of s1 and checking if any of them is a substring of s2. This is computationally expensive, especially for larger strings, because generating all permutations of s1 has factorial complexity.

Steps:
1. Generate all possible permutations of s1.
2. Check if any of these permutations is a substring of s2.
3. Return true if a match is found, otherwise return false.
##### Time Complexity:
O(n! * m), where n is the length of s1 and m is the length of s2. This approach generates all permutations of s1, each of which is checked in s2.
##### Space Complexity:
O(n! + m) for storing all permutations and searching in s2.
##### Python Code:
```python
from itertools import permutations

def checkInclusion(s1: str, s2: str) -> bool:
    perms = permutations(s1)  # Generate all permutations of s1
    for p in perms:
        if ''.join(p) in s2:
            return True
    return False
```
### Approach 2: Sliding Window with Frequency Maps (Optimal)
##### Intuition: 
A more efficient solution can be achieved using the sliding window technique combined with frequency maps. The idea is to check if any substring of s2 has the same character frequency as s1, which would indicate that the substring is a permutation of s1.

1. Use a frequency map for s1 to count the occurrence of each character.
2. Slide a window of length len(s1) across s2 and maintain a frequency map for the current window in s2.
3. Compare the frequency maps for s1 and the current window in s2. If they match, the window contains a permutation of s1.

Steps:
1. Create a frequency map for s1 and the first len(s1) characters of s2.
2. Slide the window across s2:
   - Add the next character to the window and remove the first character of the previous window.
   - Update the frequency map accordingly.
   - Compare the frequency maps for s1 and the current window.
3. If a match is found, return true. If the loop completes without a match, return false.
##### Visualization:
```perl
For s1 = "ab" and s2 = "eidbaooo":

Initial window: "ei" → Frequency map does not match "ab"
Slide window:
  Window: "id" → No match
  Window: "db" → No match
  Window: "ba" → Match found (Permutation of "ab")
Return True
```
##### Time Complexity:
O(m + n), where m is the length of s2 and n is the length of s1. We traverse both strings and update the frequency maps in constant time.
##### Space Complexity:
O(1), since the frequency maps contain at most 26 characters (for the lowercase alphabet).
##### Python Code:
```python
def checkInclusion(s1: str, s2: str) -> bool:
    from collections import Counter
    
    n, m = len(s1), len(s2)
    if n > m:
        return False
    
    s1_count = Counter(s1)  # Frequency map for s1
    window_count = Counter(s2[:n])  # Frequency map for the first window in s2
    
    if s1_count == window_count:
        return True
    
    # Slide the window over s2
    for i in range(n, m):
        window_count[s2[i]] += 1  # Add new character to window
        window_count[s2[i - n]] -= 1  # Remove the first character of the previous window
        
        # Remove character count from window if it becomes 0
        if window_count[s2[i - n]] == 0:
            del window_count[s2[i - n]]
        
        if s1_count == window_count:
            return True
    
    return False
```
### Approach 3: Sliding Window with Single Array (Further Optimized)
##### Intuition: 
Instead of using the Counter class, we can optimize the solution further by using two arrays of size 26 (since there are 26 lowercase English letters). One array will store the frequency of characters in s1, and the other will store the frequency of the current window in s2. This allows for more efficient comparison between frequency maps.

1. Create two arrays s1_count and window_count of size 26 to store the frequency of characters in s1 and the current window of s2.
2. Use the sliding window approach to move across s2, updating the window_count array.
3. Compare the two arrays at each step to see if the current window in s2 matches the permutation of s1.

Steps:
1. Initialize two frequency arrays s1_count and window_count with size 26.
2. Fill s1_count with the frequency of characters in s1 and the first window of size len(s1) in s2.
3. Slide the window across s2, updating the frequency array and checking for matches.
##### Visualization:
```perl
For s1 = "abc" and s2 = "eidbaooo":

Initial frequency arrays:
  s1_count: [1, 1, 1, 0, ...]
  window_count: [1, 1, 0, 0, ...]

Slide window:
  Update window_count → No match
Continue sliding and checking until a match is found or window reaches the end.
```
##### Time Complexity:
O(m), where m is the length of s2. We traverse s2 once and update the frequency arrays in constant time.
##### Space Complexity:
O(1), since the space complexity depends only on the fixed size of the frequency array (26 for lowercase letters).
##### Python Code:
```python
def checkInclusion(s1: str, s2: str) -> bool:
    if len(s1) > len(s2):
        return False
    
    # Frequency arrays
    s1_count = [0] * 26
    window_count = [0] * 26
    
    for i in range(len(s1)):
        s1_count[ord(s1[i]) - ord('a')] += 1
        window_count[ord(s2[i]) - ord('a')] += 1
    
    # Slide the window over s2
    for i in range(len(s1), len(s2)):
        if s1_count == window_count:
            return True
        
        # Add the new character to the window
        window_count[ord(s2[i]) - ord('a')] += 1
        # Remove the old character from the window
        window_count[ord(s2[i - len(s1)]) - ord('a')] -= 1
    
    return s1_count == window_count
```
## Summary
| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Brute Force	                    | 	O(n! * m)      | O(n! + m)             |
| Sliding Window with Frequency Maps		        | O(m + n)	            | O(1)             |
| Sliding Window with Single Array		              | O(m)            | O(1)             |

The Sliding Window with Single Array approach is the most efficient solution, processing the input string in linear time and using constant space.