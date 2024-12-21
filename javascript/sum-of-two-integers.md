# [Leetcode 371: Sum of Two Integers](https://leetcode.com/problems/sum-of-two-integers/)

## Approaches
1. [Using Bit Manipulation with Loop](#approach-1)
2. [Using Bit Manipulation with Recursion](#approach-2)

---

## Approach 1: Using Bit Manipulation with Loop

### Intuition
The problem requires us to sum two integers without using the '+' or '-' operators. The idea is to use bit manipulation to achieve this. The sum can be derived using the XOR operator, and carrying over bits can be derived using the AND operator followed by a left shift.

### Explanation
- The XOR operator `a ^ b` gives us the sum of `a` and `b` only without carrying.
- The carry, i.e., where both bits are 1, is captured using `a & b` and shifting it left by 1.
- We repeat this process until there's no carry left.
- This process mimics how we manually perform addition with carrying over digits.

Here's the implementation for this approach:

```javascript
var getSum = function(a, b) {
    while (b !== 0) {
        // Calculate the carry, which is where both a and b have 1 at the same bit position
        let carry = a & b;
        
        // Calculate the non-carrying sum using XOR
        a = a ^ b;
        
        // Shift carry left by 1 to add in the next higher order
        b = carry << 1;
    }
    return a;
};
```

### Time Complexity
- **O(N)** where N is the number of bits in the integer. The worst case involves looping over all bits.
  
### Space Complexity
- **O(1)** as we are using a constant amount of extra space.

---

## Approach 2: Using Bit Manipulation with Recursion

### Intuition
This approach leverages the same bit manipulation logic as above. However, instead of using an iterative process, it employs recursion to keep updating the numbers `a` and `b` until there's no carry.

### Explanation
- Similar to the iterative version, calculate `a ^ b` (sum without carry) and `(a & b) << 1` (carry).
- Recursively call the function with these new values until carry becomes zero.

Here's the implementation for the recursive approach:

```javascript
var getSum = function(a, b) {
    if (b === 0) return a; // Base case: if no carry left, return a
    return getSum(a ^ b, (a & b) << 1); // Recursive step with sum and carry
};
```

### Time Complexity
- **O(N)** similar to the iterative approach, where N is the number of bits in the integer due to potential depth of recursive calls.
  
### Space Complexity
- **O(N)** for the recursive stack, as it takes stack space equivalent to the number of bits in the input integer.

Each approach effectively utilizes bit manipulation to simulate the addition of two numbers without using arithmetic operators. By understanding how XOR and AND operations can mimic addition and carrying in binary numbers, we achieve the solution efficiently.

