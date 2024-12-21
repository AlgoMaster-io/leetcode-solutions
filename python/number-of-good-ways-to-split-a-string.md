# [Leetcode 1525: Number of Good Ways to Split a String](https://leetcode.com/problems/number-of-good-ways-to-split-a-string/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Prefix and Suffix Frequency Arrays](#optimized-prefix-and-suffix-frequency-arrays)

---

### Brute Force Approach

Intuition:
The problem is essentially asking us to determine how many ways we can partition the string `s` such that both halves contain the same number of unique characters. A straightforward method to do this is to iterate over all possible split points; for each split point, count unique characters in the `left` and `right` substrings.

- **Time Complexity:** \(O(n^2)\) - Due to iterating over all possible partitions and counting unique characters for each partition.
- **Space Complexity:** \(O(n)\) - To store unique characters for the left and right parts using sets.

```python
def numSplits(s: str) -> int:
    # Initialize the result to count the number of good splits.
    result = 0
    n = len(s)

    # Traverse through each possible splitting point.
    for i in range(1, n):
        # Use sets to count unique characters in left and right parts.
        left_unique = set(s[:i])
        right_unique = set(s[i:])

        # Compare if both sets have the same size (unique character counts).
        if len(left_unique) == len(right_unique):
            result += 1
    
    return result
```

---

### Optimized Prefix and Suffix Frequency Arrays

Intuition:
Instead of recalculating the unique character counts for each potential split, we can precompute these values using prefix and suffix arrays. By iterating over the string once to construct these arrays, we can quickly determine the number of unique characters on the left and right sides of any potential split.

- **Time Complexity:** \(O(n)\) - Constructing the prefix and suffix arrays in linear time.
- **Space Complexity:** \(O(26 + 26) = O(1)\) - Since the size of the alphabet is constant (for lowercase English letters).

```python
from collections import defaultdict

def numSplits(s: str) -> int:
    # This will track unique character counts from the left.
    left_count = [0] * len(s)
    # This will track unique character counts from the right.
    right_count = [0] * len(s)

    # A dictionary to count occurrences of each character for prefixes.
    prefix_char_count = defaultdict(int)
    # A dictionary to count occurrences of each character for suffixes.
    suffix_char_count = defaultdict(int)
    
    # Compute the unique character counts from the left.
    unique_chars_left = 0
    for i in range(len(s)):
        char = s[i]
        if prefix_char_count[char] == 0:
            unique_chars_left += 1
        prefix_char_count[char] += 1
        left_count[i] = unique_chars_left
    
    # Compute the unique character counts from the right.
    unique_chars_right = 0
    for i in range(len(s) - 1, -1, -1):
        char = s[i]
        if suffix_char_count[char] == 0:
            unique_chars_right += 1
        suffix_char_count[char] += 1
        right_count[i] = unique_chars_right

    # Compare left and right unique character counts to find good splits.
    result = 0
    for i in range(1, len(s)):
        if left_count[i - 1] == right_count[i]:
            result += 1
    
    return result
```

Each block of code in this solution highlights the approach we've used; from computing unique character counts using two traversals of the input array to using these precomputed values to make quick checks for valid string partitions. This represents an optimal balancing of the problem's time and space complexity constraints.

