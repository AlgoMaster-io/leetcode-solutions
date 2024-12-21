
# [Leetcode 438: Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

## Approaches:
- [Approach 1: Brute-force Checking](#approach-1-brute-force-checking)
- [Approach 2: Sorting and HashMap](#approach-2-sorting-and-hashmap)
- [Approach 3: Efficient Sliding Window with HashMap](#approach-3-efficient-sliding-window-with-hashmap)

---

## Approach 1: Brute-force Checking

### Intuition
The simplest approach to solve this problem is to generate all substrings of length `len(p)` in string `s`, and check if they are anagrams of the string `p`.

### Code

```python
def findAnagrams(s, p):
    # Resultant list to store starting indices of the anagrams
    result = []
    # Sort the pattern string once as a reference
    sorted_p = sorted(p)
    
    # Loop over each possible starting point for a substring of length `len(p)`
    for i in range(len(s) - len(p) + 1):
        # Extract the substring of the same length as p
        substring = s[i:i+len(p)]
        # Check if the sorted substring matches the sorted pattern
        if sorted(substring) == sorted_p:
            result.append(i)
    
    return result

# Example usage
findAnagrams("cbaebabacd", "abc")
```

### Time Complexity
- The time complexity of this approach is O((n - m) * m * log(m)), where n is the length of `s` and m is the length of `p`. This is because for each substring, sorting takes O(m log m).
  
### Space Complexity
- The space complexity is O(m) for storing the sorted version of `p`.

---

## Approach 2: Sorting and HashMap

### Intuition
Instead of sorting every substring and `p` to compare them, hash maps (or frequency dictionaries) can be used to improve the performance. By comparing the frequency counts of characters, we determine if the two strings are anagrams.

### Code

```python
from collections import Counter

def findAnagrams(s, p):
    result = []
    p_count = Counter(p)
    # Initialize a counter for the sliding window portion of s
    s_count = Counter(s[:len(p) - 1])

    # Use a sliding window to check each substring of length `len(p)`
    for i in range(len(p) - 1, len(s)):
        # Add the new character to the current window counter
        s_count[s[i]] += 1
        # If the window counter matches the pattern counter, an anagram is found
        if s_count == p_count:
            result.append(i - len(p) + 1)
        # Remove the oldest character from the window counter
        s_count[s[i - len(p) + 1]] -= 1
        # Clean up zero-count keys from the dictionary
        if s_count[s[i - len(p) + 1]] == 0:
            del s_count[s[i - len(p) + 1]]

    return result

# Example usage
findAnagrams("cbaebabacd", "abc")
```

### Time Complexity
- O(n), because we only make a single pass over `s` with constant time operations on a fixed character set size.

### Space Complexity
- O(1), relative to the size of the character set (which is typically fixed).

---

## Approach 3: Efficient Sliding Window with HashMap

### Intuition
This approach uses a sliding window combined with a hash map to keep track of character counts efficiently. The goal is to eliminate the need to recompute the frequency map for each new substring.

### Code

```python
def findAnagrams(s, p):
    # Initialize result and dictionary structures
    result = []
    p_count = Counter(p)
    s_count = Counter()

    # Use sliding window
    for i in range(len(s)):
        # Add current character
        s_count[s[i]] += 1
        
        # Remove character outside the window
        if i >= len(p):
            if s_count[s[i - len(p)]] == 1:
                del s_count[s[i - len(p)]]
            else:
                s_count[s[i - len(p)]] -= 1
        
        # Compare dictionaries, add start index to results if an anagram is found
        if p_count == s_count:
            result.append(i - len(p) + 1)
        
    return result

# Example usage
findAnagrams("cbaebabacd", "abc")
```

### Time Complexity
- O(n), as it iterates through the main string only once using a sliding window.

### Space Complexity
- O(1), as the space used by the Counter is constant relative to the fixed character set (assuming only lowercase letters).

