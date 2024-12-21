[Leetcode Problem 214: Shortest Palindrome](https://leetcode.com/problems/shortest-palindrome/)

### Table of Contents
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: KMP Algorithm](#approach-2-kmp-algorithm)

---

### Approach 1: Brute Force
The brute force solution involves checking for the longest palindromic prefix of the string. Once we find it, we can reflect the rest of the string and add it to the start.

#### Intuition:
1. A palindrome reads the same forward and backward, so the primary step is to find the longest palindromic prefix of the string.
2. With the longest palindromic prefix, the remainder of the string can be reversed and prepended to the start, forming the shortest palindrome.

#### Steps:
- Iterate from the end of the string, and for each position, check if the substring from start to that position is a palindrome.
- Once found, reverse the non-palindromic suffix and prepend it to the original string.

#### Time Complexity:
- O(n^2) where n is the length of the string due to the palindrome check for each substring.

#### Space Complexity:
- O(n) for storing the reversed string.

```go
func shortestPalindromeBruteForce(s string) string {
    // Helper function to check if a string is a palindrome.
    isPalindrome := func(s string) bool {
        n := len(s)
        // Compare characters from start and end moving towards the center.
        for i := 0; i < n / 2; i++ {
            if s[i] != s[n - i - 1] {
                return false
            }
        }
        return true
    }
    
    n := len(s)
    // Check palindromic prefix iteratively from the full string reducing.
    for i := n; i >= 0; i-- {
        if isPalindrome(s[:i]) {
            // Once a palindromic prefix is found, reverse and prepend the suffix.
            suffix := s[i:]
            // Convert to byte slice to reverse
            reversed := make([]byte, len(suffix))
            for j, ch := range suffix {
                reversed[len(suffix)-j-1] = byte(ch)
            }
            return string(reversed) + s
        }
    }
    return ""
}
```

---

### Approach 2: KMP Algorithm
Apply the KMP (Knuth-Morris-Pratt) string matching algorithm’s partial match table (pi/longest prefix suffix array) to achieve a more optimal solution.

#### Intuition:
1. Concatenate the original string with its reverse with a separator.
2. Use the KMP table to find the longest palindromic suffix, giving information about the longest palindromic prefix of the original string.

#### Steps:
- Build a new string from `s + "#" + reversed(s)`.
- Compute the LPS (Longest Prefix which is also a Suffix) array for this new string.
- The value at the last index of the LPS array gives the length of the longest palindrome starting at index 0 of `s`.

#### Time Complexity:
- O(n) where n is the length of the string, as we are using KMP preprocessing effectively.

#### Space Complexity:
- O(n) for storing the LPS array.

```go
func shortestPalindromeKMP(s string) string {
    // Concatenate the original string and its reverse separated by a special character.
    rev := reverseString(s)
    l := s + "#" + rev
    n := len(l)
    
    // Build the KMP partial match table (LPS array).
    lps := make([]int, n)
    
    length := 0
    i := 1
    
    for i < n {
        if l[i] == l[length] {
            length++
            lps[i] = length
            i++
        } else {
            if length != 0 {
                length = lps[length-1]
            } else {
                lps[i] = 0
                i++
            }
        }
    }
    
    // Find the substring to be added to make the original a palindrome.
    add := rev[:len(s)-lps[n-1]]
    return add + s
}

func reverseString(s string) string {
    b := []byte(s)
    for i, j := 0, len(b)-1; i < j; i, j = i+1, j-1 {
        b[i], b[j] = b[j], b[i]
    }
    return string(b)
}
```

In conclusion, the KMP-based approach significantly optimizes the solution by reducing time complexity effectively compared to the brute force approach by leveraging the structure of the string efficiently.

