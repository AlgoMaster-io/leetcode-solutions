# [Leetcode 14: Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)

## Approaches:
1. [Horizontal Scanning](#horizontal-scanning)
2. [Vertical Scanning](#vertical-scanning)
3. [Divie and Conquer](#divide-and-conquer)
4. [Binary Search](#binary-search)

### Approach 1: Horizontal Scanning

The idea behind horizontal scanning is to look at the prefix common to the first two strings and then continue with this "common prefix" and the next string, and so on.

#### Intuition:
- Start by considering the entire first string as the initial common prefix.
- Compare this prefix with the next string, and adjust the length of the prefix if necessary.
- Repeat the process for all strings in the array.

#### Code:

```python
def longestCommonPrefix_horizontal(strs):
    if not strs:
        return ""
    
    # Start with the full first string as a prefix
    prefix = strs[0]
    
    # Compare the prefix with each string one by one
    for s in strs[1:]:
        # Reduce the prefix size until it matches the beginning of `s`
        while not s.startswith(prefix):
            prefix = prefix[:-1]  # Remove the last character from the prefix
            if not prefix:  # If prefix becomes empty, no common prefix
                return ""
    
    return prefix
```

#### Complexity:
- **Time Complexity**: O(S), where S is the sum of all characters in all strings.
- **Space Complexity**: O(1) as no additional space is used.

### Approach 2: Vertical Scanning

In this approach, we check if the character at each position in all strings is the same.

#### Intuition:
- Compare the characters one by one vertically down the rows.
- Stop when a mismatch is found or when any string's end is reached.

#### Code:

```python
def longestCommonPrefix_vertical(strs):
    if not strs:
        return ""
    
    # Check each column in each row
    for i in range(len(strs[0])):
        char = strs[0][i]
        
        for s in strs[1:]:
            # If the current string length is less than i or characters mismatch
            if i >= len(s) or s[i] != char:
                return strs[0][:i]
    
    return strs[0]
```

#### Complexity:
- **Time Complexity**: O(S), where S is the sum of all characters in all strings.
- **Space Complexity**: O(1) as no additional space is used.

### Approach 3: Divide and Conquer

This method applies the divide and conquer paradigm.

#### Intuition:
- Divide the problem into two subproblems and merge the result.
- The "merge" operation finds the common prefix between two strings.

#### Code:

```python
def longestCommonPrefix_divide_and_conquer(strs):
    if not strs:
        return ""
    
    def common_prefix(left, right):
        min_len = min(len(left), len(right))
        for i in range(min_len):
            if left[i] != right[i]:
                return left[:i]
        return left[:min_len]
    
    def longest_common_prefix_recursive(l, r):
        if l == r:
            return strs[l]
        else:
            # Divide the list into two halves
            mid = (l + r) // 2
            lcp_left = longest_common_prefix_recursive(l, mid)
            lcp_right = longest_common_prefix_recursive(mid + 1, r)
            # Conquer/Combine step
            return common_prefix(lcp_left, lcp_right)
    
    return longest_common_prefix_recursive(0, len(strs) - 1)
```

#### Complexity:
- **Time Complexity**: O(S), where S is the sum of all characters in all strings.
- **Space Complexity**: O(m * log(n)) for the recursive call stack, where m is the length of the shortest string and n is the number of strings.

### Approach 4: Binary Search

Use binary search to find the length of the common prefix.

#### Intuition:
- Perform a binary search on the length of the common prefix.
- Check if a prefix of this length is common to all strings.

#### Code:

```python
def longestCommonPrefix_binary_search(strs):
    if not strs:
        return ""
    
    # Find the minimum length string in strs
    min_length = min(len(s) for s in strs)
    
    low, high = 0, min_length
    
    def is_common_prefix(length):
        prefix = strs[0][:length]
        return all(s.startswith(prefix) for s in strs[1:])
    
    while low <= high:
        mid = (low + high) // 2
        if is_common_prefix(mid):
            low = mid + 1
        else:
            high = mid - 1
    
    return strs[0][:(low + high) // 2]

```

#### Complexity:
- **Time Complexity**: O(S * log(minLen)), where S is the sum of all characters in all strings and minLen is the length of the shortest string in strs.
- **Space Complexity**: O(1) as no additional space is used.

