# Leetcode Problem: [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

## Approaches
1. [Brute Force](#approach-1-brute-force)
2. [Sliding Window with Set](#approach-2-sliding-window-with-set)
3. [Sliding Window with HashMap and Optimal Complexity](#approach-3-sliding-window-with-hashmap-and-optimal-complexity)

---

## Approach 1: Brute Force

### Intuition
The simplest approach involves checking every possible substring and determining if it contains all unique characters. This requires two nested loops: the outer loop to fix the starting character and the inner loop to consider all substrings starting from this character. For every substring, we can use a set to keep track of characters and validate uniqueness.

### Time Complexity
- **Time:** O(n^3) due to generating all possible substrings and validating uniqueness for each.
- **Space:** O(min(n, m)) where `m` is the character set size (typically 26 for lowercase English letters) for the set used to verify uniqueness.

### Solution
```python
def lengthOfLongestSubstring(s: str) -> int:
    n = len(s)
    max_length = 0
    
    # Iterate over each possible starting index
    for i in range(n):
        seen_chars = set()
        current_length = 0
        
        # Check each possible ending index for the substring starting at i
        for j in range(i, n):
            if s[j] in seen_chars:
                break
            seen_chars.add(s[j])
            current_length += 1
        
        # Update the maximum length found
        max_length = max(max_length, current_length)
    
    return max_length
```

---

## Approach 2: Sliding Window with Set

### Intuition
Instead of checking every possible substring, we can use the sliding window technique with two pointers. The goal is to maintain a window `[i, j)` (where `j` is not inclusive) of unique characters. If a duplicate character is found at `j`, we move the start of the window (`i`) to one past the first occurrence of the duplicate character.

### Time Complexity
- **Time:** O(2n) = O(n). In the worst case, each character will be visited twice.
- **Space:** O(min(n, m)) for the set to maintain unique characters window.

### Solution
```python
def lengthOfLongestSubstring(s: str) -> int:
    n = len(s)
    max_length = 0
    seen_chars = set()
    i = 0  # Left pointer of the window
    
    # Iterate over each character with j as the right pointer
    for j in range(n):
        # Slide i to the right until duplicate character is removed
        while s[j] in seen_chars:
            seen_chars.remove(s[i])
            i += 1
        seen_chars.add(s[j])
        max_length = max(max_length, j - i + 1)
    
    return max_length
```

---

## Approach 3: Sliding Window with HashMap and Optimal Complexity

### Intuition
Enhancing the sliding window approach, we use a hashmap (dictionary) to store the last seen index of each character. When a duplicate character is encountered, we can jump the left pointer directly to the index after the last occurrence of this duplicate, thereby optimizing window advancement.

### Time Complexity
- **Time:** O(n) because each character is processed at most twice (once in and once out of the window).
- **Space:** O(min(n, m)) where `m` is the character set size for the hashmap storing indices.

### Solution
```python
def lengthOfLongestSubstring(s: str) -> int:
    char_index_map = {}
    max_length = 0
    i = 0  # Start of the current window
    
    # Traverse the string with j as the right boundary of the window
    for j, char in enumerate(s):
        # If the character is already in the map, move the start to i + 1 after its last occurrence
        if char in char_index_map:
            i = max(char_index_map[char] + 1, i)
        
        # Update the current window length
        max_length = max(max_length, j - i + 1)
        
        # Record the index of the current character
        char_index_map[char] = j
    
    return max_length
```

Using these approaches, we can iteratively optimize the solution from a brute force attempt to a linear time complexity solution utilizing the sliding window technique efficiently.

