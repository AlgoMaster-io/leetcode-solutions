# [Leetcode 424: Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)

## Approaches
1. [Brute Force](#brute-force)
2. [Sliding Window Approach](#sliding-window-approach)

## Brute Force

### Intuition
The brute force solution involves checking every possible substring of the given string and calculating if the substring can be transformed to a uniform string by replacing at most `k` characters. It is non-optimal due to the need to check each substring.

### Solution
- Iterate over all possible substrings of the original string.
- For each substring, count the frequency of characters.
- Replace the remaining characters (substring's length minus max frequency) and check if replacements are within `k`.
- Update maximum length if the condition is satisfied.

### Time Complexity
- O(n^3) - Because we are iterating over all substrings and calculating character frequencies for each.

### Space Complexity
- O(1) - Using fixed extra space for counting characters.

```go
func characterReplacement(s string, k int) int {
    maxLength := 0
    n := len(s)
    
    // Iterate through all possible substrings
    for i := 0; i < n; i++ {
        // Use a map to count character frequencies
        freqMap := make(map[byte]int)
        maxFreq := 0
        
        for j := i; j < n; j++ {
            char := s[j]
            freqMap[char]++
            
            // Maintain the count of the most frequent character
            if freqMap[char] > maxFreq {
                maxFreq = freqMap[char]
            }
            
            // Calculate the current substring's length
            currentLength := j - i + 1
            
            // If the current length minus max frequency is within k, update maxLength
            if currentLength - maxFreq <= k {
                if currentLength > maxLength {
                    maxLength = currentLength
                }
            }
        }
    }
    
    return maxLength
}
```

## Sliding Window Approach

### Intuition
Instead of examining every possible substring, the sliding window approach lets us efficiently keep track of the window length where replacements are allowed. We expand the window when possibilities allow and contract it when constraints are broken. This approach requires maintaining the frequency of characters within the current window.

### Solution
- Use two pointers to define a window, both starting at the beginning of the string.
- Use a map to hold the frequency of each character within the window.
- Expand the window by moving the right pointer, updating the frequency map.
- Calculate the potential length of transformation and maintain maximum length.
- Contract the window when condition `(right - left + 1 - maxFreq)` exceeds `k`.

### Time Complexity
- O(n) - Because each character is processed at most twice (once by the right pointer and at most once by the left pointer).

### Space Complexity
- O(1) - We use fixed extra space for the frequency map.

```go
func characterReplacement(s string, k int) int {
    maxLength := 0
    left := 0
    maxFreq := 0
    freqMap := make(map[byte]int)
    
    for right := 0; right < len(s); right++ {
        // Include current character in the window
        char := s[right]
        freqMap[char]++
        
        // Update the maximum frequency of any character in the current window
        if freqMap[char] > maxFreq {
            maxFreq = freqMap[char]
        }
        
        // If the current window is invalid, move the left pointer
        if right - left + 1 - maxFreq > k {
            freqMap[s[left]]--
            left++
        }
        
        // Update the maximum length of a valid window
        if right - left + 1 > maxLength {
            maxLength = right - left + 1
        }
    }
    
    return maxLength
}
```

With the sliding window approach, we achieve an optimal solution with linear complexity, suitable for handling longer strings efficiently.

