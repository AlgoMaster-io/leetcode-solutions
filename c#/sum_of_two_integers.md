# 371. [Sum of Two Integers](https://leetcode.com/problems/sum-of-two-integers/)

## Approach 1: Bit Manipulation (Iterative)

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(1)

public class Solution {
    public int GetSum(int a, int b) {
        while (b != 0) {
            int carry = a & b; // Calculate carry bits
            a = a ^ b; // Calculate sum without carry
            b = carry << 1; // Shift carry to the left
        }
        return a; // When b becomes zero, a contains the sum
    }
}
```

## Approach 2: Bit Manipulation (Recursive)

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n) due to recursion stack

public class Solution {
    public int GetSum(int a, int b) {
        if (b == 0) {
            return a; // Base case: no carry left
        }
        return GetSum(a ^ b, (a & b) << 1); // Recursive call with sum and carry
    }
}
```

These solutions utilize bit manipulation to handle addition without using the '+' operator. The `^` operator is used to add, and the `&` operator is used to determine carry bits. The carry is shifted left to add to the next bit position.
