# [Leetcode 371: Sum of Two Integers](https://leetcode.com/problems/sum-of-two-integers/)

## Approaches
- [Approach 1: Simulate Addition using Bitwise Operations](#approach-1-simulate-addition-using-bitwise-operations)
- [Approach 2: Using Iterative Bit Manipulation](#approach-2-using-iterative-bit-manipulation)

## Approach 1: Simulate Addition using Bitwise Operations

**Intuition**: 
To sum two numbers without using the `+` operator, we leverage bit manipulation. When bits are summed, the result is similar to binary addition:
- Sum bits (`a XOR b`) for current position without carry.
- Calculate the carry (`(a AND b) << 1`) because it affects the next bit position.
- The new value of `a` becomes the result of (a XOR b).
- The new value of `b` becomes the carry until b is 0.

**Code**:
```cpp
class Solution {
public:
    int getSum(int a, int b) {
        // Process until there is no carry
        while (b != 0) {
            // Carry now contains the common set bits of a and b
            unsigned carry = a & b;
            
            // Sum of bits of a and b where at least one of the bits is not set
            a = a ^ b;
            
            // Carry is shifted left so that adding it to a gives the required sum
            b = carry << 1;
        }
        
        // When b becomes 0, a contains the sum result
        return a;
    }
};
```

**Time Complexity**: O(1)  
In practice, this algorithm runs in constant time, as the number of bits in an integer is fixed (e.g., 32 bits).

**Space Complexity**: O(1)


## Approach 2: Using Iterative Bit Manipulation

**Intuition**: 
Instead of using loops, recursion can be used to repeatedly achieve the result following the same logic.

**Code**:
```cpp
class Solution {
public:
    int getSum(int a, int b) {
        if (b == 0)
            return a;
        
        // Recursion will continue to handle carry until it's zero
        return getSum(a ^ b, (a & b) << 1);
    }
};
```

**Time Complexity**: O(1)  
As with the iterative loop method, the function runs in constant time due to limited integer size.

**Space Complexity**: O(1)  
Recursive stack depth is fixed due to limited integer width.

