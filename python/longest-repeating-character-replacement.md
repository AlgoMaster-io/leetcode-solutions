```markdown
# [Leetcode 424: Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)

## Approaches:
1. [Brute Force Approach](#approach-1)
2. [Sliding Window with Fixed Window Size](#approach-2)
3. [Optimal Sliding Window](#approach-3)

---

## Approach 1: Brute Force

### Intuition
The brute force solution considers every possible substring, computes the frequency of each character in that substring, and determines if it can be transformed into a string with all same characters with at most `k` replacements.

### Solution
```python
def characterReplacement(s: str, k: int) -> int:
    max_length = 0
    n = len(s)
    
    for i in range(n):
        for j in range(i, n):
            # Create a frequency dictionary for each substring
            freq = {}
            for char in s[i:j+1]:
                if char not in freq:
                    freq[char] = 0
                freq[char] += 1
            
            # Determine the maximum frequency of a single character in the substring
            max_freq = max(freq.values())
            
            # Check if it's possible within k replacements
            if (j - i + 1) - max_freq <= k:
                max_length = max(max_length, j - i + 1)
    
    return max_length
```

### Time Complexity
- **O(n^3)** where `n` is the length of the string (due to the nested loops and the frequency count inside the inner loop).

### Space Complexity
- **O(1)** (ignoring the space taken by the frequency dictionary which is bounded by at most 26 keys for lowercase alphabets).

---

## Approach 2: Sliding Window with Fixed Window Size

### Intuition
Fix a window size and use a sliding window technique to check if replacing less or equal to `k` characters in the current window can make all characters the same.

### Solution
```python
def characterReplacement(s: str, k: int) -> int:
    max_length = 0
    n = len(s)
    
    for window_size in range(1, n + 1):
        max_freq = 0
        freq = {}
        for i in range(n):
            # Add current character to the frequency map
            if s[i] not in freq:
                freq[s[i]] = 0
            freq[s[i]] += 1
            if i - window_size >= 0:
                freq[s[i - window_size]] -= 1
                if freq[s[i - window_size]] == 0:
                    del freq[s[i - window_size]]
            
            max_freq = max(max_freq, freq[s[i]])
            
            # If the rest characters except the max frequency's character can be replaced
            if (window_size - max_freq) <= k:
                max_length = max(max_length, window_size)
    
    return max_length
```

### Time Complexity
- **O(n^2)** as it involves keeping track of character frequencies for each possible window size using sliding window.

### Space Complexity
- **O(1)**, similar to the previous approach.

---

## Approach 3: Optimal Sliding Window

### Intuition
Instead of fixing the window size, expand the window dynamically. Use the sliding window to find the longest valid substring by checking if it is possible to convert it into a uniform string with remaining `k` character replacements.

### Solution
```python
def characterReplacement(s: str, k: int) -> int:
    max_length = 0
    max_freq = 0
    freq = {}
    left = 0
    
    for right in range(len(s)):
        # Add the current character to the frequency dictionary or update its frequency
        char = s[right]
        if char not in freq:
            freq[char] = 0
        freq[char] += 1
        
        # Update max frequency character in the current window
        max_freq = max(max_freq, freq[char])
        
        # If the current window exceeds allowable replacements, shrink it
        while (right - left + 1) - max_freq > k:
            freq[s[left]] -= 1
            left += 1
        
        # Calculate the max length for the valid window
        max_length = max(max_length, right - left + 1)
        
    return max_length
```

### Time Complexity
- **O(n)** since each character is processed at most twice.

### Space Complexity
- **O(1)** - the space for the frequency map is bounded by a fixed size (26 lowercase letters).

---
```

This markdown provides detailed explanations and code snippets for each approach, with comments and complexity analysis included to help understand the thought process behind each solution.

