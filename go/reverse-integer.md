# [Leetcode 7: Reverse Integer](https://leetcode.com/problems/reverse-integer/)

## Solutions:
- [Solution 1: String Manipulation](#solution-1-string-manipulation)
- [Solution 2: Mathematical Approach](#solution-2-mathematical-approach)
- [Solution 3: Optimized Mathematical Approach](#solution-3-optimized-mathematical-approach)

### Solution 1: String Manipulation

**Intuition:**

For the simplest solution, we can convert the integer to a string, reverse this string, and then convert it back to an integer. This is a straightforward approach that is easy to implement but not the most efficient.

**Implementation:**

```go
func reverseStringManipulation(x int) int {
    isNegative := x < 0
    
    // Convert the integer to a string
    s := strconv.Itoa(x)
    if isNegative {
        s = s[1:] // Remove the negative sign if it exists
    }
    
    // Reverse the string
    runes := []rune(s)
    for i, j := 0, len(runes)-1; i < j; i, j = i+1, j-1 {
        runes[i], runes[j] = runes[j], runes[i]
    }
    revStr := string(runes)
    
    // Convert the reversed string back to an integer
    revInt, err := strconv.Atoi(revStr)
    if err != nil {
        return 0
    }
    
    if isNegative {
        revInt = -revInt
    }
    
    // Check for 32-bit overflow
    if revInt < -1<<31 || revInt > 1<<31-1 {
        return 0
    }
    
    return revInt
}
```

**Complexity Analysis:**
- Time Complexity: O(n), where n is the number of digits in the number. This is due to the conversion to string and reversal.
- Space Complexity: O(n), where n is due to the storage of the string representation of the number.

### Solution 2: Mathematical Approach

**Intuition:**

Instead of converting the integer to a string, we can leverage mathematics to reverse the number. By repeatedly extracting the last digit using the modulus operation, we can reconstruct the reversed number.

**Implementation:**

```go
func reverseMath(x int) int {
    rev := 0
    for x != 0 {
        digit := x % 10
        x /= 10
        rev = rev*10 + digit
    }
    
    // Check for 32-bit overflow
    if rev < -1<<31 || rev > 1<<31-1 {
        return 0
    }
    
    return rev
}
```

**Complexity Analysis:**
- Time Complexity: O(n), where n is the number of digits in the number.
- Space Complexity: O(1), as no additional space is used beyond the output.

### Solution 3: Optimized Mathematical Approach

**Intuition:**

We can enhance the mathematical reversal approach by checking for overflow before doing the multiplication by 10. This prevents the result from exceeding the 32-bit signed integer range.

**Implementation:**

```go
func reverseOptimizedMath(x int) int {
    rev := 0
    for x != 0 {
        digit := x % 10
        x /= 10
        // Check for positive overflow
        if rev > (1<<31-1-digit)/10 {
            return 0
        }
        // Check for negative overflow
        if rev < (-1<<31-digit)/10 {
            return 0
        }
        rev = rev*10 + digit
    }
    
    return rev
}
```

**Complexity Analysis:**
- Time Complexity: O(n), where n is the number of digits in the number.
- Space Complexity: O(1), using constant space.

