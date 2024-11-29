# [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

## Approach 1: Brute Force (Basic)

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(n)
package main

func lengthOfLongestSubstring(s string) int {
    maxLength := 0

    // Check every possible substring
    for i := 0; i < len(s); i++ {
        seen := make(map[rune]bool)
        length := 0

        for j := i; j < len(s); j++ {
            if seen[rune(s[j])] {
                break // Break if a duplicate character is found
            }
            seen[rune(s[j])] = true
            length++
            if length > maxLength {
                maxLength = length // Update maxLength
            }
        }
    }

    return maxLength
}
```

## Approach 2: Sliding Window with HashSet

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
package main

func lengthOfLongestSubstring(s string) int {
    set := make(map[rune]bool)
    maxLength, left := 0, 0

    // Sliding window
    for right := 0; right < len(s); right++ {
        for set[rune(s[right])] {
            delete(set, rune(s[left])) // Shrink the window
            left++
        }
        set[rune(s[right])] = true // Expand the window
        if right-left+1 > maxLength {
            maxLength = right - left + 1 // Update maxLength
        }
    }

    return maxLength
}
```

## Approach 3: Sliding Window with HashMap (Optimal)

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
package main

func lengthOfLongestSubstring(s string) int {
    charIndexMap := make(map[rune]int)
    maxLength, left := 0, 0

    // Sliding window
    for right, ch := range s {
        
        if idx, found := charIndexMap[ch]; found {
            if idx+1 > left {
                left = idx + 1 // If the character is already in the map, update the left pointer
            }
        }

        // Update the character's last index in the map
        charIndexMap[ch] = right

        // Update maxLength
        if right-left+1 > maxLength {
            maxLength = right - left + 1
        }
    }

    return maxLength
}
```

