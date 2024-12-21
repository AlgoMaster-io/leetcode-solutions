# [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

## Approaches
- [Brute Force](#brute-force)
- [Sliding Window](#sliding-window)
- [Optimized Sliding Window with HashMap](#optimized-sliding-window-with-hashmap)

---

### Brute Force

The brute force approach involves generating all substrings and checking if they contain all unique characters. Although simple, this approach is inefficient for long strings due to its high time complexity.

#### Intuition:
- Generate all possible substrings.
- Check each substring to see if it contains all unique characters.
- Keep a track of the maximum length found so far.

```go
func lengthOfLongestSubstring(s string) int {
    // Function to determine if the substring contains all unique characters
    hasUniqueChars := func(s string) bool {
        charSet := make(map[rune]bool)
        for _, char := range s {
            if charSet[char] {
                return false
            }
            charSet[char] = true
        }
        return true
    }
    
    maxLength := 0
    n := len(s)
    
    // Iterate over all possible substrings
    for i := 0; i < n; i++ {
        for j := i + 1; j <= n; j++ {
            if hasUniqueChars(s[i:j]) {
                maxLength = max(maxLength, j-i)
            }
        }
    }
    
    return maxLength
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

**Time Complexity:** O(n^3)  
**Space Complexity:** O(n), due to the character set.

---

### Sliding Window

The sliding window approach refines the brute force method. Instead of checking all substrings, it adjusts the starting and ending indices to ensure characters remain unique.

#### Intuition:
- Use two pointers (window) to track the current substring.
- Expand the window until a duplicate character is found.
- Move the start of the window to remove the duplicate, and continue.

```go
func lengthOfLongestSubstring(s string) int {
    n := len(s)
    maxLength := 0
    start := 0
    charSet := make(map[rune]bool)
    
    for end, char := range s {
        // If we encounter a duplicate character, move the start pointer
        for charSet[char] {
            delete(charSet, rune(s[start]))
            start++
        }
        // Add the current character to the set
        charSet[char] = true
        // Update the maximum length
        maxLength = max(maxLength, end-start+1)
    }
    
    return maxLength
}
```

**Time Complexity:** O(2n) ≈ O(n)  
**Space Complexity:** O(min(n, m)), where m is the size of the charset/alphabet.

---

### Optimized Sliding Window with HashMap

Further optimize the sliding window using a hashmap to store the latest index of each character, allowing for efficient window movement.

#### Intuition:
- Instead of moving the `start` pointer one step at a time (as in the earlier sliding window approach), jump directly to the index after the last occurrence of the repeated character.
- This reduces unnecessary movements of the sliding window.

```go
func lengthOfLongestSubstring(s string) int {
    n := len(s)
    maxLength := 0
    start := 0
    charIndex := make(map[rune]int)
    
    for end, char := range s {
        // If the character is already in the map and its index is equal or after start
        if lastIdx, found := charIndex[char]; found && lastIdx >= start {
            // Move start to the index after the last occurrence of the current character
            start = lastIdx + 1
        }
        // Update the character's latest index
        charIndex[char] = end
        // Calculate maxLength for the current window
        maxLength = max(maxLength, end-start+1)
    }
    
    return maxLength
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(min(n, m)), where m is the size of the charset/alphabet.

---

Each approach above progressively refines the logic by reducing the redundant checks and improving runtime efficiency using specific data structures.

