# [Leetcode 371: Sum of Two Integers](https://leetcode.com/problems/sum-of-two-integers/)

## Approaches
1. [Basic Approach: Using Bit Manipulation with Iterative Addition](#basic-approach)
2. [Optimal Approach: Bit Manipulation with Recursion](#optimal-approach)

### Basic Approach: Using Bit Manipulation with Iterative Addition
The idea of this problem is to add two integers without using the `+` or `-` operators. We can achieve this by utilizing bit manipulation and iterative logic.

Intuition:
- The key to solving this problem is understanding how addition works at the bit level.
- We use XOR to determine the sum of the bits not considering the carry. If there is no carry, XOR will give us the correct bit.
- We use AND followed by a left shift to determine the carry.
- We repeat this process until there is no carry left.

```go
func getSum(a int, b int) int {
    // We loop until there's no carry left to add
    for b != 0 {
        // The carry is calculated using AND and shifting the result
        carry := a & b
        // The result without carry is calculated using XOR
        a = a ^ b
        // Update 'b' with the carry, shifted by 1 to add in the next iteration
        b = carry << 1
    }
    // 'a' contains the sum
    return a
}
```

**Time Complexity:** O(1) - The loop runs a constant number of times (32 times in the worst case for a 32-bit integer).

**Space Complexity:** O(1) - No extra space is used other than variables.

### Optimal Approach: Bit Manipulation with Recursion
We can optimize the process using recursion, which naturally handles the loop mechanism for updating our result by repeatedly adding the carry.

Intuition:
- This approach follows the same logic as the iterative method but uses recursion to handle the looping process.
- The XOR operation is used to find the sum without considering the carry.
- The AND operation is used, followed by a shift, to identify the carry which needs to be added.

```go
func getSum(a int, b int) int {
    if b == 0 {
        // Base case: no carry left to add
        return a
    }
    // Recursive step: sum without carry, add carry in next recursive call
    return getSum(a^b, (a&b)<<1)
}
```

**Time Complexity:** O(1) - This is recursive, but effectively still operates in constant time due to the fixed number of possible carries.

**Space Complexity:** O(1) - Recursion stack space is constant and limited to the number of bits in an integer.

Both methods essentially achieve the same result using similar principles of bit manipulation, but the recursive approach is concise and elegant, effectively breaking down the addition into simpler recursive calls.

