# [567. Permutation in String](https://leetcode.com/problems/permutation-in-string/)

## Approach 1: Brute Force (Basic)

### Solution
```go
// Time Complexity: O(n * m), where n is the length of s1 and m is the length of s2
// Space Complexity: O(n)
import (
    "sort"
    "strings"
)

func checkInclusion(s1 string, s2 string) bool {
    n, m := len(s1), len(s2)

    if n > m {
        return false
    }

    // Sort s1
    s1Arr := strings.Split(s1, "")
    sort.Strings(s1Arr)
    sortedS1 := strings.Join(s1Arr, "")

    // Check every substring of length n in s2
    for i := 0; i <= m-n; i++ {
        subArr := strings.Split(s2[i:i+n], "")
        sort.Strings(subArr)
        if sortedS1 == strings.Join(subArr, "") {
            return true
        }
    }

    return false
}
```

## Approach 2: Sliding Window with Frequency Array (Optimal)

### Solution
```go
// Time Complexity: O(m), where m is the length of s2
// Space Complexity: O(1)
func checkInclusion(s1 string, s2 string) bool {
    if len(s1) > len(s2) {
        return false
    }

    s1Freq := make([]int, 26)
    s2Freq := make([]int, 26)

    // Frequency count for s1
    for _, c := range s1 {
        s1Freq[c-'a']++
    }

    // Sliding window
    for i := 0; i < len(s2); i++ {
        s2Freq[s2[i]-'a']++

        // Remove the leftmost character from the window
        if i >= len(s1) {
            s2Freq[s2[i-len(s1)]-'a']--
        }

        // Compare the frequency arrays
        if equal(s1Freq, s2Freq) {
            return true
        }
    }

    return false
}

// Helper function to compare two frequency arrays
func equal(arr1, arr2 []int) bool {
    for i := 0; i < 26; i++ {
        if arr1[i] != arr2[i] {
            return false
        }
    }
    return true
}
```

