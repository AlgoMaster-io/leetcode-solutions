# [Leetcode 392: Is Subsequence](https://leetcode.com/problems/is-subsequence/)

## Approaches

1. [Two Pointer Approach](#two-pointer-approach)
2. [Follow-up: Followed Approach with Binary Search](#follow-up-followed-approach-with-binary-search)

---

## Two Pointer Approach

### Intuition

The idea behind this approach is straightforward. We have two strings, `s` and `t`. Our goal is to check if `s` is a subsequence of `t`. To do so, we can utilize two pointers. One pointer will iterate over string `s` and the other pointer will iterate over string `t`. We move the pointer on `s` only when the characters of both `s` and `t` match. The pointer on `t` always moves forward. If by the end of `t`, we have moved the pointer of `s` to the end, it means `s` is a subsequence of `t`.

### Detailed Steps

1. Initialize two pointers: `pointer_s` starting at 0 for string `s` and `pointer_t` starting at 0 for string `t`.
2. Traverse through string `t` using `pointer_t`.
3. If `s[pointer_s]` equals `t[pointer_t]`, move `pointer_s` to the next position.
4. Always increment `pointer_t` to move through string `t`.
5. If `pointer_s` reaches the length of `s` after the iteration, `s` is a subsequence of `t`.
6. If you finish iterating through `t` without finishing `s`, then `s` is not a subsequence.

### Code

```python
def isSubsequence(s: str, t: str) -> bool:
    pointer_s, pointer_t = 0, 0
    
    while pointer_s < len(s) and pointer_t < len(t):
        # Check if current character in both strings match
        if s[pointer_s] == t[pointer_t]:
            # Move the pointer for string s
            pointer_s += 1
        # Always move the pointer for string t
        pointer_t += 1
    
    # If we've traversed all of s, it's a subsequence of t
    return pointer_s == len(s)
```

### Complexity Analysis

- **Time Complexity**: O(n) - where n is the length of string `t`. We iterate through `t` only once.
- **Space Complexity**: O(1) - We use a constant amount of extra space for pointers.

---

## Follow-up: Followed Approach with Binary Search

### Intuition

In the case of multiple queries where we need to check if a sequence is a subsequence of a longer string `t`, optimizing the search can improve efficiency. We preprocess string `t` to store the indices of each character, allowing us to use binary search to quickly find if there is a valid subsequence.

### Detailed Steps

1. Preprocess the string `t` to create a dictionary where each character maps to a list of indices where it appears.
2. For each character in `s`, check its presence in `t` by searching in the indices list for that character.
3. Use binary search to find the smallest index in `t` that is greater than the index of the previous character in `s`.
4. If you can traverse `s` successfully in `t` using this method, `s` is a subsequence of `t`.

### Code

```python
from bisect import bisect_left
from collections import defaultdict

def isSubsequence(s: str, t: str) -> bool:
    # Preprocess t: map each char in t to the list of indices it appears at
    char_indices = defaultdict(list)
    for index, char in enumerate(t):
        char_indices[char].append(index)
    
    # Previous character index in t
    prev_index = -1
    for char in s:
        # Get the list of all indices for the current character
        indices = char_indices[char]
        # Find the least index which is greater than prev_index
        # Equivalent to getting the next occurrence of char in t
        position = bisect_left(indices, prev_index + 1)
        if position == len(indices):
            # If we cannot find a valid subsequence index, return False
            return False
        
        # Update prev_index to the current valid index found
        prev_index = indices[position]
    
    return True
```

### Complexity Analysis

- **Time Complexity**: 
  - Preprocessing: O(n), where n is the length of `t`.
  - Query: O(m log k), where m is the length of `s` and k is the average number of appearances of a character in `t`.
- **Space Complexity**: O(n), for storing the index list for each character.

