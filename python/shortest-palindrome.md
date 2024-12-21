# [Shortest Palindrome](https://leetcode.com/problems/shortest-palindrome/)

## Solutions

- [Brute Force Approach](#brute-force-approach)
- [KMP (Knuth-Morris-Pratt) Algorithm](#kmp-algorithm)

---

### Brute Force Approach

#### Intuition
The brute force approach involves iteratively checking substrings to find the longest prefix that is also a palindrome. The straightforward way is to reverse the string and check if the prefix of the original equals the suffix of the reversed string. This solution works by detecting this longest palindromic prefix, and then appending the corresponding suffix to make the full string a palindrome.

#### Algorithm
1. Reverse the string and store it in a variable `rev_s`.
2. Iterate over the string `s`:
   - Check if the substring from the start to index `i` in `s` is equal to the substring from the end to index `len(s)-i-1` in `rev_s`.
   - Once you find a matching prefix, break the loop.
3. Combine the non-palindrome part of `s` with `s` itself by attaching it as a prefix.

#### Code
```python
def shortest_palindrome(s: str) -> str:
    if not s:
        return s

    rev_s = s[::-1]
    
    # Check the prefix being the longest palindrome
    for i in range(len(s) + 1):
        if s.startswith(rev_s[i:]):
            return rev_s[:i] + s
    return ""

```

#### Complexity
- **Time Complexity**: \(O(n^2)\), where \(n\) is the length of the input string 's'. In the worst case, we scan each character for each possible prefix.
- **Space Complexity**: \(O(n)\) for creating the reversed string.

---

### KMP Algorithm

#### Intuition
The KMP algorithm is used to preprocess the given string and its reverse, thus providing an efficient way to find the longest prefix, which is a palindrome. This approach makes use of the "partial match" table of the KMP algorithm to determine this longest palindromic prefix. 

#### Algorithm
1. Concatenate the original string `s`, a separator (which doesn't appear in `s`), and the reversed string `rev_s`.
2. Compute the `lps` (Longest Prefix Suffix) array for this concatenated string.
3. The value at `lps[-1]` provides the length of the longest palindromic prefix.
4. Use this to append the necessary characters from the reverse string to make the string a palindrome.

#### Code
```python
def shortest_palindrome(s: str) -> str:
    if not s:
        return s
    
    # Create a new string which is the original + separator + reversed
    new_s = s + "#" + s[::-1]
    
    # Compute LPS array for this new concatenated string
    lps = compute_lps(new_s)
    
    # Calculate the part that needs to be appended by excluding the longest prefix
    to_add = s[lps[-1]:][::-1]  # Add in reversed order
    
    return to_add + s

def compute_lps(s: str) -> [int]:
    # longest prefix which is also suffix
    lps = [0] * len(s)
    length = 0  # length of the previous longest prefix suffix
    i = 1
    
    while i < len(s):
        if s[i] == s[length]:
            length += 1
            lps[i] = length
            i += 1
        else:
            if length != 0:
                length = lps[length - 1]
            else:
                lps[i] = 0
                i += 1
    return lps
```

#### Complexity
- **Time Complexity**: \(O(n)\), where \(n\) is the combined length of the original and reversed string. The LPS computation is linear.
- **Space Complexity**: \(O(n)\) due to the space required for the `lps` array.

