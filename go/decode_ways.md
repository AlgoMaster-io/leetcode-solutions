# 91. [Decode Ways](https://leetcode.com/problems/decode-ways/)

## Approach 1: Recursion with Memoization

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
package main

func numDecodings(s string) int {
    memo := make(map[int]int)
    return decodeHelper(s, 0, memo)
}

func decodeHelper(s string, index int, memo map[int]int) int {
    // If we've reached the end of the string, return 1 for a valid path
    if index == len(s) {
        return 1
    }
    
    // If the current portion of the string starts with '0', it's invalid
    if s[index] == '0' {
        return 0
    }
    
    // If we've already computed the result for this index, return it
    if val, found := memo[index]; found {
        return val
    }
    
    // Decode one character
    result := decodeHelper(s, index+1, memo)
    
    // Decode two characters if it's a valid two-digit number
    if index+1 < len(s) && (s[index] == '1' || (s[index] == '2' && s[index+1] < '7')) {
        result += decodeHelper(s, index+2, memo)
    }
    
    // Save the result to the memoization map
    memo[index] = result
    
    return result
}
```

## Approach 2: Dynamic Programming

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
package main

func numDecodings(s string) int {
    if len(s) == 0 || s[0] == '0' {
        return 0
    }
    
    n := len(s)
    dp := make([]int, n+1)
    
    dp[0] = 1  // Base case: an empty string has one way to decode
    dp[1] = 1  // Base case: the first character can't be '0'
    
    for i := 2; i <= n; i++ {
        oneDigit := int(s[i-1] - '0')
        twoDigits := int((s[i-2]-'0')*10 + (s[i-1] - '0'))
        
        if oneDigit >= 1 && oneDigit <= 9 {
            dp[i] += dp[i-1]
        }
        
        if twoDigits >= 10 && twoDigits <= 26 {
            dp[i] += dp[i-2]
        }
    }
    
    return dp[n]
}
```

## Approach 3: Dynamic Programming with Space Optimization

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
package main

func numDecodings(s string) int {
    if len(s) == 0 || s[0] == '0' {
        return 0
    }

    n := len(s)
    prev2 := 1 // dp[i-2]
    prev1 := 1 // dp[i-1]
    
    for i := 1; i < n; i++ {
        current := 0
        oneDigit := int(s[i] - '0')
        twoDigits := int((s[i-1]-'0')*10 + (s[i] - '0'))
        
        if oneDigit >= 1 && oneDigit <= 9 {
            current += prev1
        }
        
        if twoDigits >= 10 && twoDigits <= 26 {
            current += prev2
        }
        
        prev2 = prev1
        prev1 = current
    }
    
    return prev1
}
```

