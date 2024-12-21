# [Leetcode 76: Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

## Approaches

1. [Sliding Window with Two Pointers (Brute Force)](#approach-1-sliding-window-with-two-pointers-brute-force)
2. [Sliding Window with HashMap (Optimized)](#approach-2-sliding-window-with-hashmap-optimized)

---

## Approach 1: Sliding Window with Two Pointers (Brute Force)

### Intuition
The problem involves finding the smallest substring in `s` that contains all characters of `t`. A simple idea is to explore every possible substring of `s` and check if it fulfills the requirement of containing all characters of `t`. This method is not efficient but is straightforward due to its exhaustive nature.

### Approach
1. **Iterate** over all possible starting points in `s`.
2. For each starting point, **extend** the second pointer to find a substring that contains all characters of `t`.
3. **Check** if the substring satisfies the need for all characters of `t`.
4. If it does, **compare** with the minimum window found so far and update if this one is smaller.
5. Return the smallest window found.

### Code

```go
func minWindow(s string, t string) string {
    // Dictionary which keeps a count of all the unique characters in t.
    dictT := map[rune]int{}
    for _, char := range t {
        dictT[char]++
    }
    
    totalUniqueChars := len(dictT) // Number of unique characters in t.
    minLength := len(s) + 1 
    minWindow := ""
    
    for i := 0; i < len(s); i++ { 
        // Create a hashmap to count characters for the current window.
        windowCounts := map[rune]int{} 
        formed := 0 // Matches number of unique characters currently formed.
        
        for j := i; j < len(s); j++ { 
            char := rune(s[j])
            windowCounts[char]++
            
            // Check if current character meets the need from dictT.
            if count, exists := dictT[char]; exists && windowCounts[char] == count {
                formed++
            }
            
            // When window contains all needed chars.
            if formed == totalUniqueChars {
                if j-i+1 < minLength {
                    minLength = j-i+1
                    minWindow = s[i : j+1]
                }
                break // No need to expand further for this i
            }
        }
    }
    
    return minWindow
}
```

### Complexity Analysis
- **Time Complexity**: \(O(n^2 + m)\), where \(n\) is the length of `s` and \(m\) is the length of `t`.
- **Space Complexity**: \(O(m)\) due to maintaining `dictT` and `windowCounts`.

## Approach 2: Sliding Window with HashMap (Optimized)

### Intuition
Instead of examining every possible substring, we can maintain a sliding window using two pointers and dynamically adjust the window to satisfy the condition of containing all characters of `t`. This improves efficiency by avoiding redundant checks and reducing unnecessary recalculations.

### Approach
1. Use a hashmap to store the character frequency of `t`.
2. Use two pointers for the sliding window, `left` and `right`, starting at the beginning of `s`.
3. Expand `right` to include characters until the window satisfies the required condition.
4. Contract `left` to minimize the window while maintaining the condition.
5. Throughout, track the minimum valid window found.
6. Return the minimum window once both pointers have traversed `s`.

### Code

```go
func minWindow(s string, t string) string {
    if len(s) == 0 || len(t) == 0 {
        return ""
    }
    
    // Dictionary to store count of all unique characters in t.
    dictT := map[rune]int{}
    for _, char := range t {
        dictT[char]++
    }
    
    l, r := 0, 0
    required := len(dictT)
    formed := 0
    windowCounts := map[rune]int{}
    
    minLength := len(s) + 1
    minLeft, minRight := 0, 0
    
    for r < len(s) {
        char := rune(s[r])
        windowCounts[char]++
        
        // Count how many unique characters in current window satisfy frequency in dictT
        if _, exists := dictT[char]; exists && windowCounts[char] == dictT[char] {
            formed++
        }
        
        // Contract from the left to find smaller windows
        for l <= r && formed == required {
            char = rune(s[l])
            if (r - l + 1) < minLength {
                minLeft = l
                minRight = r
                minLength = minRight - minLeft + 1
            }
            
            windowCounts[char]--
            if _, exists := dictT[char]; exists && windowCounts[char] < dictT[char] {
                formed--
            }
            
            l++
        }
        
        r++
    }
    
    if minLength == len(s)+1 {
        return ""
    }
    
    return s[minLeft:minRight+1]
}
```

### Complexity Analysis
- **Time Complexity**: \(O(n + m)\), where \(n\) is the length of `s` and \(m\) is the length of `t`, as each character is processed at most twice.
- **Space Complexity**: \(O(m)\) due to maintaining `dictT` and `windowCounts`. 

Both approaches lay the foundation of understanding the sliding window paradigm, essential for solving many string-related problems efficiently.

