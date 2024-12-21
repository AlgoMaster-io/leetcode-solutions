# [1044. Longest Duplicate Substring](https://leetcode.com/problems/longest-duplicate-substring)

## Approaches

1. [Brute Force with substring generation](#approach-1-brute-force-with-substring-generation)
2. [Binary Search with Rabin-Karp algorithm and Hash Set](#approach-2-binary-search-with-rabin-karp-and-set)

### Approach 1: Brute Force with substring generation

#### Intuition:
The brute force approach involves generating all possible substrings of the input string, checking each one for duplicates, and keeping track of the longest one found. This method is inefficient but provides a clear solution to the problem.

#### Implementation:
```python
def longestDupSubstring(s: str) -> str:
    def has_duplicate_substring(n):
        # This function checks if there is a duplicate substring of length n
        seen = set()
        for i in range(len(s) - n + 1):
            substring = s[i:i+n]  # Get substring from index i to i+n
            if substring in seen:
                return True
            seen.add(substring)
        return False

    for length in range(len(s) - 1, 0, -1):
        if has_duplicate_substring(length):
            for start in range(len(s) - length + 1):
                substring = s[start:start + length]
                if s.count(substring) > 1:
                    return substring
    return ""

```

#### Time Complexity:
- O(n^3): Because for each substring length, we're generating all possible substrings and checking for duplicates.

#### Space Complexity:
- O(n^2): Storing substrings in the worst case requires this space when dealing with long strings.

### Approach 2: Binary Search with Rabin-Karp and Hash Set

#### Intuition:
To optimize, use binary search over the possible lengths of the substrings (1 to n), applying the Rabin-Karp algorithm (a rolling hash method) to efficiently detect duplicates for a given length. We leverage hash sets to manage seen hashes, and check for collisions to determine duplicates.

#### Implementation:
```python
def longestDupSubstring(s: str) -> str:
    import functools

    def rabin_karp(mid):
        # Base for the rolling hash function
        a = 26
        # Modulus value for preventing integer overflow
        modulus = 2**63 - 1

        h = 0
        # Compute the hash of the first 'mid' length substring
        for i in range(mid):
            h = (h * a + nums[i]) % modulus
        
        # Store the hash of the first 'mid-length' substring
        seen = {h}
        # Value of a^mid % modulus
        aL = pow(a, mid, modulus)
        
        for start in range(1, len(s) - mid + 1):
            # Roll the hash - remove the leftmost character and add the rightmost 
            h = (h * a - nums[start - 1] * aL + nums[start + mid - 1]) % modulus
            if h in seen:
                return start
            seen.add(h)
        return -1

    # 1 to len(s)
    nums = [ord(c) - ord('a') for c in s]
    left, right = 1, len(s)
    start = 0
    while left <= right:
        mid = (left + right) // 2
        pos = rabin_karp(mid)
        if pos != -1:
            # If found, update start and explore for longer substrings
            left = mid + 1
            start = pos
        else:
            # Try shorter substrings
            right = mid - 1
    return s[start:start + left - 1]

```

#### Time Complexity:
- O(n log n): Binary searching log n times, each check with Rabin-Karp is linear.

#### Space Complexity:
- O(n): Storing hash values in the set during Rabin-Karp hashing process.

