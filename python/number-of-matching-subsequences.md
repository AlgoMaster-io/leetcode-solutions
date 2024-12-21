# Leetcode Problem: [792. Number of Matching Subsequences](https://leetcode.com/problems/number-of-matching-subsequences/)

## Approaches
1. [Brute Force With Two Pointers](#approach-1-brute-force-with-two-pointers)
2. [Frequency with Maps](#approach-2-frequency-with-maps)
3. [Indexed Characters with Binary Search (Optimal)](#approach-3-indexed-characters-with-binary-search-optimal)

---

## Approach 1: Brute Force With Two Pointers

### Intuition:
The simplest method to solve this problem is to check each word in the list to see if it is a subsequence of the given string `s` by using two pointers. If a word is a subsequence of `s`, increment a count.

### Steps:
- For each word in the list, use two pointers: one for `s` and one for the word.
- Traverse string `s` with the first pointer. For each character in `s`, check if it matches the current character of the word using the second pointer.
- If the characters match, move the pointer for the word.
- If the pointer for the word reaches the end of the word, it is a subsequence.
 
### Code:
```python
def numMatchingSubseq(s, words):
    def isSubsequence(word, s):
        # Pointer for the word
        i = 0
        # Traverse through the characters in s
        for char in s:
            if i < len(word) and word[i] == char:
                # Move word pointer on match
                i += 1
            if i == len(word):
                return True
        return i == len(word)
    
    count = 0
    for word in words:
        if isSubsequence(word, s):
            count += 1
    return count
```

### Complexity:
- **Time Complexity:** O(n * m), where `n` is the length of the input string `s`, and `m` is the accumulated length of words in the list.
- **Space Complexity:** O(1) (apart from input storage).

---

## Approach 2: Frequency with Maps

### Intuition:
Instead of scanning the entire string `s` for each word, store the positions of each character in `s`. For each word, check if its characters appear in increasing order using the stored positions.

### Steps:
- Maintain a dictionary where each character in `s` maps to a list of its indices in the string.
- For each word, attempt to match each character using the stored indices, ensuring the indices are strictly increasing with each character match.

### Code:
```python
from collections import defaultdict
from bisect import bisect_left

def numMatchingSubseq(s, words):
    char_indices = defaultdict(list)
    
    # Populate the mapping of characters to their indices
    for index, char in enumerate(s):
        char_indices[char].append(index)
    
    count = 0
    for word in words:
        prev_index = -1
        found = True
        
        for char in word:
            if char not in char_indices:
                found = False
                break
            
            # Use binary search to find an index of char that is greater than prev_index
            next_pos = bisect_left(char_indices[char], prev_index + 1)
            if next_pos == len(char_indices[char]):
                found = False
                break
            
            # Update prev_index to the index just found
            prev_index = char_indices[char][next_pos]
        
        if found:
            count += 1
    return count
```

### Complexity:
- **Time Complexity:** O(n + sum(len(word) * log m)), where `n` is the length of string `s` and `m` is the average frequency of the most common characters in `s`.
- **Space Complexity:** O(m + n), where `m` is the distinct character count in `s`.

---

## Approach 3: Indexed Characters with Binary Search (Optimal)

### Intuition:
Combine the mapping of indices from `s` with binary search to efficiently determine the presence and order of word characters within `s`.

### Steps:
- Build a dictionary with indices for each character in `s` as seen previously.
- Use binary search to efficiently find the proper indices for characters from each word in the given map.

### Code:
```python
from collections import defaultdict
from bisect import bisect_left

def numMatchingSubseq(s, words):
    char_indices = defaultdict(list)
    
    # Store character positions
    for index, char in enumerate(s):
        char_indices[char].append(index)
    
    def isSubsequence(word):
        prev_position = -1
        for char in word:
            if char not in char_indices:
                return False
            
            # Find the smallest index greater than prev_position
            idx = bisect_left(char_indices[char], prev_position + 1)
            if idx == len(char_indices[char]):
                return False
            
            prev_position = char_indices[char][idx]
        
        return True
    
    return sum(isSubsequence(word) for word in words)
```

### Complexity:
- **Time Complexity:** O(n + w * log n), where `n` is the length of `s` and `w` is the total number of words.
- **Space Complexity:** O(n), for the character-to-indices mapping of `s`.

