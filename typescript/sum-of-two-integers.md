# [Sum of Two Integers - LeetCode 371](https://leetcode.com/problems/sum-of-two-integers/)

## Approaches
1. [Bit Manipulation Approach](#bit-manipulation-approach)

---

### Bit Manipulation Approach

#### Intuition
The problem requires performing an addition of two integers without using the '+' or '-' operators. To achieve this, we can use bitwise operations. The core idea is that adding two bits follows the pattern of the XOR operation, while carrying out the overflow follows the pattern of the AND operation shifted one bit to the left.

- **XOR (`^`)** operation can be used for adding two numbers without considering the carry. It essentially tells you the sum of corresponding bits.
- **AND (`&`)** followed by a left shift (`<<`) can be used for calculating the carry from one bit addition, as it would indicate which positions will generate a carry.

We repeat the process until no carry remains. This process naturally mimics how arithmetic is done manually but on a binary level.

#### Algorithm

1. Repeat the following until there is no carry:
   - Calculate the sum without carrying, using `a = a ^ b`.
   - Calculate the carry using `b = (a & b) << 1`.
   - Set `a` to the result from the above step.
2. Once the loop is exited, `a` contains the result of the addition of the two numbers.

#### Code 

```typescript
function getSum(a: number, b: number): number {
    while (b !== 0) {
        // Calculate the carry which is ANDed result
        let carry = (a & b) << 1;
        
        // XOR gives sum without considering the carry
        a = a ^ b;
        
        // Update b to carry
        b = carry;
    }
    return a;
}

// Testing the function
console.log(getSum(1, 2));  // Output: 3
console.log(getSum(-3, -6));  // Output: -9
console.log(getSum(5, -2));  // Output: 3
```

#### Time Complexity
- **Time complexity**: O(1) - The number size is fixed (for a 32-bit integer), so the operation takes constant time.

#### Space Complexity
- **Space complexity**: O(1) - As we only use a constant amount of space regardless of input size.

This approach takes advantage of the bitwise operations inherent in CPUs to efficiently compute the sum without using arithmetic operators, demonstrating an elegant solution leveraging binary properties.

