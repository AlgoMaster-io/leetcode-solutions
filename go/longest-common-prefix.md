# [Leetcode 14: Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)

## Approaches:

1. [Horizontal Scanning](#horizontal-scanning)
2. [Vertical Scanning](#vertical-scanning)
3. [Divide And Conquer](#divide-and-conquer)
4. [Binary Search](#binary-search)

## Horizontal Scanning

### Intuition
The idea is to iteratively find the common prefix between the strings in the array. Start with the first string as the tentative common prefix, and then compare it with the subsequent strings to update the common prefix.

### Algorithm
1. Initialize the first string as the common prefix.
2. Iterate over each string, updating the common prefix by comparing against each character of the current string.
3. If at any point the common prefix becomes empty, return an empty string.

### Implementation

```go
func longestCommonPrefix(strs []string) string {
    if len(strs) == 0 {
        return ""
    }
    prefix := strs[0]
    for i := 1; i < len(strs); i++ {
        for strings.Index(strs[i], prefix) != 0 {
            prefix = prefix[:len(prefix)-1]
            if prefix == "" {
                return ""
            }
        }
    }
    return prefix
}
```

### Time and Space Complexity
- **Time Complexity**: O(S), where S is the sum of all characters in all strings. In the worst-case scenario, we compare every character of every string.
- **Space Complexity**: O(1), as we are using only a constant amount of additional space.

## Vertical Scanning

### Intuition
Compare characters in the strings at the same position from the first to the last character. Stop if a mismatch is found.

### Algorithm
1. Start with the character at the first position of each string.
2. Compare it with all strings. If it matches with all, move to the next character.
3. Stop if a mismatch is found or all characters in the shortest string are compared.

### Implementation

```go
func longestCommonPrefix(strs []string) string {
    if len(strs) == 0 {
        return ""
    }
    
    for i := 0; i < len(strs[0]); i++ {
        c := strs[0][i]
        for j := 1; j < len(strs); j++ {
            if i == len(strs[j]) || strs[j][i] != c {
                return strs[0][:i]
            }
        }
    }
    return strs[0]
}
```

### Time and Space Complexity
- **Time Complexity**: O(S), where S is the sum of all characters in all strings. We may compare the characters up to the length of the shortest string.
- **Space Complexity**: O(1), as no additional space is used.

## Divide And Conquer

### Intuition
Divide the list of strings into two halves and find the longest common prefix for each half simultaneously. Recurse until one string is left, then return that as the prefix.

### Algorithm
1. Recursively divide the array into two halves.
2. Merge the longest common prefix of the two halves.

### Implementation

```go
func longestCommonPrefix(strs []string) string {
    if len(strs) == 0 {
        return ""
    }
    return lcp(strs, 0, len(strs)-1)
}

func lcp(strs []string, l, r int) string {
    if l == r {
        return strs[l]
    } else {
        mid := (l + r) / 2
        lcpLeft := lcp(strs, l, mid)
        lcpRight := lcp(strs, mid+1, r)
        return commonPrefix(lcpLeft, lcpRight)
    }
}

func commonPrefix(left, right string) string {
    minLength := min(len(left), len(right))
    for i := 0; i < minLength; i++ {
        if left[i] != right[i] {
            return left[:i]
        }
    }
    return left[:minLength]
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

### Time and Space Complexity
- **Time Complexity**: O(S), where S is the sum of all characters in all strings, as every character is compared once.
- **Space Complexity**: O(m * log n), with m being the length of the prefix, due to the function call stack.

## Binary Search

### Intuition
The shortest string length dictates the longest possible common prefix. We can use a binary search approach to find the length of the longest common prefix. 

### Algorithm
1. Set two pointers representing the range of possible prefix lengths, from `0` to `minLength`.
2. Use binary search to determine if a prefix of length `mid` is common among all strings.
3. Adjust the range based on the result of the prefix check.

### Implementation

```go
func longestCommonPrefix(strs []string) string {
    if len(strs) == 0 {
        return ""
    }
    
    minLen := len(strs[0])
    for _, str := range strs {
        if len(str) < minLen {
            minLen = len(str)
        }
    }
    
    low, high := 0, minLen
    for low <= high {
        mid := (low + high) / 2
        if isCommonPrefix(strs, mid) {
            low = mid + 1
        } else {
            high = mid - 1
        }
    }
    return strs[0][:(low+high)/2]
}

func isCommonPrefix(strs []string, length int) bool {
    prefix := strs[0][:length]
    for _, str := range strs {
        if len(str) < length || str[:length] != prefix {
            return false
        }
    }
    return true
}
```

### Time and Space Complexity
- **Time Complexity**: O(S * log m), where m is the length of the shortest string and S is the sum of all characters in all strings. Each comparison might check each character once.
- **Space Complexity**: O(1), since no additional space is used.

