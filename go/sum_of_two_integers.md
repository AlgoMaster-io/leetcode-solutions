# 371. [Sum of Two Integers](https://leetcode.com/problems/sum-of-two-integers/)

## Approach 1: Bit Manipulation (Iterative)

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(1)

package main

func getSum(a int, b int) int {
    for b != 0 {
        carry := a & b  // Calculate carry bits
        a = a ^ b       // Calculate sum without carry
        b = carry << 1  // Shift carry to the left
    }
    return a  // When b becomes zero, a contains the sum
}
```

## Approach 2: Bit Manipulation (Recursive)

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(n) due to recursion stack

package main

func getSum(a int, b int) int {
    if b == 0 {
        return a  // Base case: no carry left
    }
    return getSum(a^b, (a&b)<<1)  // Recursive call with sum and carry
}
```

These solutions utilize bit manipulation to handle addition without using the '+' operator. The `^` operator is used to add, and the `&` operator is used to determine carry bits. The carry is shifted left to add to the next bit position.

