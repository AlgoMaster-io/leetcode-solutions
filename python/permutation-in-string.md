# [Leetcode Problem 567: Permutation in String](https://leetcode.com/problems/permutation-in-string/)

## Table of Contents
1. [Approach 1: Generate All Permutations (Brute Force)](#approach-1-generate-all-permutations-brute-force)
2. [Approach 2: Sliding Window with Character Count Comparison](#approach-2-sliding-window-with-character-count-comparison)

---

## Approach 1: Generate All Permutations (Brute Force)

### Intuition
The simplest approach to solve this problem is to generate all possible permutations of string `s1` and check if any of these permutations is a substring of `s2`. However, generating all permutations is computationally expensive as it involves factorial time complexity.

### Implementation
```python
from itertools import permutations

def checkInclusion(s1: str, s2: str) -> bool:
    # Generate all permutations of s1
    perm = permutations(s1)
    
    # Check if any permutation is a substring of s2
    for p in perm:
        if ''.join(p) in s2:
            return True
    return False

# Example usage
s1 = "ab"
s2 = "eidbaooo"
print(checkInclusion(s1, s2))  # Output: True
```

### Time and Space Complexity
- **Time Complexity**: \(O(n! \times m)\), where \(n\) is the length of `s1`, and \(m\) is the length of `s2`.
- **Space Complexity**: \(O(n!)\), required for storing permutations.

---

## Approach 2: Sliding Window with Character Count Comparison

### Intuition
A more efficient way is to use the sliding window technique combined with character counts. The main idea is to use a fixed-size window that slides over `s2`. The length of this window should be equal to the length of `s1`. For each position of the window:
1. Check if the character frequency matches that of `s1`.
2. If frequencies match, it implies a permutation of `s1` is present in the current window position of `s2`.

### Detailed Steps
1. Initialize character count arrays for `s1` and the first window in `s2`.
2. Slide the window over `s2`:
   - Add the count of the new character entering the window.
   - Subtract the count of the character exiting the window.
   - After updating, compare the count arrays of `s1` and the current window.

### Implementation
```python
def checkInclusion(s1: str, s2: str) -> bool:
    if len(s1) > len(s2):
        return False

    # Initialize count arrays for characters
    s1_count = [0] * 26
    s2_count = [0] * 26

    # Fill the count arrays for s1 and the first window in s2
    for i in range(len(s1)):
        s1_count[ord(s1[i]) - ord('a')] += 1
        s2_count[ord(s2[i]) - ord('a')] += 1
    
    # Helper function to compare two count arrays
    def matches(s1_count, s2_count):
        for i in range(26):
            if s1_count[i] != s2_count[i]:
                return False
        return True

    # Slide through s2
    for i in range(len(s1), len(s2)):
        if matches(s1_count, s2_count):
            return True
        # Include a new character in the window
        s2_count[ord(s2[i]) - ord('a')] += 1
        # Exclude the oldest character from the window
        s2_count[ord(s2[i - len(s1)]) - ord('a')] -= 1

    # Check the last window after the loop
    return matches(s1_count, s2_count)

# Example usage
s1 = "ab"
s2 = "eidbaooo"
print(checkInclusion(s1, s2))  # Output: True
```

### Time and Space Complexity
- **Time Complexity**: \(O(l + m)\), where \(l\) is the length of `s1`, and \(m\) is the length of `s2`. Iterating once through `s2` and performing constant-time operations for each character.
- **Space Complexity**: \(O(1)\), the space needed for the frequency arrays is constant.

