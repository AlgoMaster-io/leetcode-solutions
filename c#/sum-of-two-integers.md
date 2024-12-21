# [Leetcode 371: Sum of Two Integers](https://leetcode.com/problems/sum-of-two-integers/)

## Approaches
1. [Bit Manipulation using Iteration](#approach-1-bit-manipulation-using-iteration)
2. [Bit Manipulation using Recursion](#approach-2-bit-manipulation-using-recursion)

### Approach 1: Bit Manipulation using Iteration

The problem asks us to calculate the sum of two integers a and b without using the operators `+` and `-`. To solve this, we can use bit manipulation. The idea is based on how addition works in binary:

- **XOR** of two bits can be seen as addition without the carry.
- **AND** of two bits and shifting it left gives us the carry.

We can repeat this process until there is no carry left.

#### Intuition
1. Compute the non-carry sums using XOR: `a ^ b` - This operation gives the sum of a and b bits where there is no carry.
2. Compute the carry using AND and left shift: `(a & b) << 1` - This calculates which bits need to be carried over.
3. Repeat the above steps till there are no carries left (`b == 0`).

#### Code

```csharp
public class Solution {
    public int GetSum(int a, int b) {
        while (b != 0) {
            // Carry now contains common set bits of a and b
            int carry = a & b;

            // Sum of bits of a and b where at least one of the bits is not set
            a = a ^ b;

            // Carry is shifted by one so that adding it to a gives the required sum
            b = carry << 1;
        }

        return a;
    }
}
```

#### Complexity Analysis
- **Time Complexity**: O(1) - The loop runs a fixed number of times since integers are 32-bits.
- **Space Complexity**: O(1) - We are using a constant amount of additional space.

### Approach 2: Bit Manipulation using Recursion

This approach is similar to the iterative version, but instead of using a loop, we use recursion to achieve the same operations until the carry is zero.

#### Intuition
1. Similar to the iterative version, this uses XOR and AND operations but handles each step recursively.
2. Depending on a base condition (carry being zero), it calls itself to continue applying the carry to the sum obtained by XOR.

#### Code

```csharp
public class Solution {
    public int GetSum(int a, int b) {
        if (b == 0) return a; // When there is no carry left
        int sum = a ^ b; // Sum without considering carry
        int carry = (a & b) << 1; // Calculate the carry
        return GetSum(sum, carry); // Recursive call
    }
}
```

#### Complexity Analysis
- **Time Complexity**: O(1) - Like the iterative approach, the recursion depth is bounded by the number of bits, so it is constant time.
- **Space Complexity**: O(1) - Since this is a tail-recursive function (though stack space is used, it is still considered O(1) in practical recursive scenarios).

Both approaches exhibit efficient and elegant usage of bit manipulation to solve the problem without using arithmetic operators.

