# 214. [Shortest Palindrome](https://leetcode.com/problems/shortest-palindrome/)

## Approach 1: Brute Force Reverse and Check

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(n)
func shortestPalindrome(s string) string {
    // Edge case for empty string
    if len(s) == 0 {
        return s
    }

    // Reverse the string
    rev := reverseString(s)

    // Try to find a palindrome by checking each position
    for i := 0; i < len(s); i++ {
        // Check if the substring of s and reversed s is palindrome
        if s[:len(s)-i] == rev[i:] {
            return rev[:i] + s
        }
    }

    return "" // This will never be executed
}

func reverseString(s string) string {
    runes := []rune(s)
    for i, j := 0, len(runes)-1; i < j; i, j = i+1, j-1 {
        runes[i], runes[j] = runes[j], runes[i]
    }
    return string(runes)
}
```

## Approach 2: KMP Algorithm

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
func shortestPalindrome(s string) string {
    rev := reverseString(s)
    combined := s + "#" + rev

    // Compute KMP table
    kmpTable := computeKMPTable(combined)

    // The palindrome length from using kmpTable
    palindromeLength := kmpTable[len(combined)-1]

    // Append the part missing to form a palindrome
    return rev[:len(rev)-palindromeLength] + s
}

func reverseString(s string) string {
    runes := []rune(s)
    for i, j := 0, len(runes)-1; i < j; i, j = i+1, j-1 {
        runes[i], runes[j] = runes[j], runes[i]
    }
    return string(runes)
}

func computeKMPTable(s string) []int {
    kmpTable := make([]int, len(s))
    j := 0

    for i := 1; i < len(s); i++ {
        for j > 0 && s[i] != s[j] {
            j = kmpTable[j-1]
        }
        if s[i] == s[j] {
            j++
        }
        kmpTable[i] = j
    }

    return kmpTable
}
```

