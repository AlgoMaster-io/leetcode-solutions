# [Leetcode 371: Sum of Two Integers](https://leetcode.com/problems/sum-of-two-integers/)

## Approaches
1. [Iterative Bit Manipulation](#iterative-bit-manipulation)
2. [Recursive Bit Manipulation](#recursive-bit-manipulation)

---

### Approach 1: Iterative Bit Manipulation

The problem states that we need to sum two integers without using the operators `+` or `-`. This can be achieved through bit manipulation using the concepts of XOR and carry. Here's the step-by-step breakdown:

- **XOR operation (`a ^ b`)**: This operation is like addition but ignores the carry. If only one of the bits is set (either in `a` or in `b`, but not both), it will be set in the result.

- **AND operation (`a & b`)**: This helps to determine where there would be a carry. When both bits are set (in `a` and `b`), a carry is generated.

- **Left shift operation (`<<`)**: After identifying the carry bits using `AND`, they need to be shifted left to add them at the next higher bit position.

- Repeat this process until there are no carries left to process.

#### Code
```java
public int getSum(int a, int b) {
    // Continue the loop until there are no carries left
    while (b != 0) {
        // Calculate the carry
        int carry = a & b;
        // Calculate sum ignoring the carry
        a = a ^ b;
        // Update the carry, shifted left
        b = carry << 1;
    }
    // Finally, a contains the sum
    return a;
}
```

#### Time Complexity
- The time complexity is O(1), because the number of bits is fixed, the loop will iterate a finite number of times.

#### Space Complexity
- The space complexity is O(1), as no additional space is used.

---

### Approach 2: Recursive Bit Manipulation

This approach uses the same logic as the iterative approach but utilizes recursion instead.

#### Code
```java
public int getSum(int a, int b) {
    // Base case: when there are no more carries to process
    if (b == 0) return a;
    // Calculate sum ignoring carries, and calculate the carry
    return getSum(a ^ b, (a & b) << 1);
}
```

#### Time Complexity
- The time complexity is O(1) since the recursion depth is limited by the number of bits in an integer.

#### Space Complexity
- The space complexity is O(1) if we consider just the computation. However, there is a recursive call stack used which would typically be O(n) where n is the number of bits, but due to limits in integer size, it is effectively constant.

---

Both approaches leverage fundamental concepts of computer arithmetic using bit manipulation, showcasing an elegant solution to the problem without the need for typical arithmetic operations.

