# [Leetcode 91: Decode Ways](https://leetcode.com/problems/decode-ways/)

## Approaches
- [Recursive Approach](#recursive-approach)
- [Dynamic Programming Approach](#dynamic-programming-approach)
- [Dynamic Programming Optimized Approach](#dynamic-programming-optimized-approach)

### Recursive Approach

The problem can be solved using a simple recursive method where we iterate over each character, trying to match it with either one or two characters in the string. The recursion checks at each step if the current number (1 character or 2 characters) can be decoded into a valid letter. 

#### Intuition:
1. Iterate through the string.
2. If the current digit is a valid single character encoding (1-9), recurse to solve the rest of the string.
3. If the current and next digits form a valid two-character encoding (10-26), recurse to solve the rest of the string skipping an additional character.

#### Time Complexity:
- O(2^n), where n is the length of the string. This happens because, from each character, we try two paths (one or two characters).

#### Space Complexity:
- O(n) due to recursion stack.

```go
func numDecodings(s string) int {
    return helper(s, 0)
}

func helper(s string, index int) int {
    if index == len(s) {
        return 1
    }
    
    if s[index] == '0' {
        return 0
    }
    
    count := helper(s, index + 1)
    
    if index+1 < len(s) && (s[index] == '1' || (s[index] == '2' && s[index+1] < '7')) {
        count += helper(s, index + 2)
    }
    
    return count
}
```

### Dynamic Programming Approach

This approach uses an array to store previously computed results to avoid repeated calculations. It essentially builds from the base case—an empty string or reaching the end of the string is one valid decoding.

#### Intuition:
1. Create a dp array where dp[i] represents the number of ways to decode the substring of s[0...i-1].
2. If a single digit at position i is valid (not '0'), increment dp[i].
3. If a two-digit number formed with digits at position i-1 and i is valid, update dp[i] with the number of ways to decode it.

#### Time Complexity:
- O(n), where n is the length of the string, as this approach computes each state once.

#### Space Complexity:
- O(n) for the dp array.

```go
func numDecodings(s string) int {
    if len(s) == 0 {
        return 0
    }
    
    dp := make([]int, len(s)+1)
    dp[0] = 1
    dp[1] = 0
    
    if s[0] != '0' {
        dp[1] = 1
    }
    
    for i := 2; i <= len(s); i++ {
        singleDigit, _ := strconv.Atoi(s[i-1:i])
        doubleDigits, _ := strconv.Atoi(s[i-2:i])
        
        if singleDigit >= 1 {
            dp[i] += dp[i-1]
        }
        
        if doubleDigits >= 10 && doubleDigits <= 26 {
            dp[i] += dp[i-2]
        }
    }
    
    return dp[len(s)]
}
```

### Dynamic Programming Optimized Approach

This method optimizes space by using only two variables instead of an entire array.

#### Intuition:
1. Use two variables to keep track of the previous two dp states.
2. Update these variables as you progress through the string.
3. The concept is similar to Fibonacci sequence space optimization.

#### Time Complexity:
- O(n), where n is the length of the string.

#### Space Complexity:
- O(1) as we are using only a fixed amount of extra space.

```go
func numDecodings(s string) int {
    if len(s) == 0 || s[0] == '0' {
        return 0
    }

    prev2 := 1
    prev1 := 1

    for i := 1; i < len(s); i++ {
        current := 0
        
        if s[i] != '0' {
            current = prev1
        }
        
        num, _ := strconv.Atoi(s[i-1:i+1])
        if num >= 10 && num <= 26 {
            current += prev2
        }
        
        prev2 = prev1
        prev1 = current
    }
    
    return prev1
}
```

Each approach builds upon the previous, starting with a simple recursive solution and optimizing towards the most space-efficient dynamic programming strategy.

