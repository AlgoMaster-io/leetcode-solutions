# 191. [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/)

## Approach 1: Loop and Bit Manipulation

### Solution
csharp
```csharp
// Time Complexity: O(32) because an integer in C# is 32 bits.
// Space Complexity: O(1)
public class Solution {
    public int HammingWeight(uint n) {
        int count = 0;
        while (n != 0) {
            // Check if the least significant bit is 1
            count += (int)(n & 1);
            // Right shift to process the next bit
            n >>= 1;
        }
        return count;
    }
}
```

## Approach 2: Brian Kernighan’s Algorithm

### Solution
csharp
```csharp
// Time Complexity: O(1) in the sense that the operation is related to the number of 1s.
// Space Complexity: O(1)
public class Solution {
    public int HammingWeight(uint n) {
        int count = 0;
        while (n != 0) {
            // Drop the lowest set bits
            n &= (n - 1);
            // Increment count for each bit cleared
            count++;
        }
        return count;
    }
}
```

## Approach 3: Using Built-in Function

### Solution
csharp
```csharp
// Time Complexity: O(1) as the function call is executing in a constant time.
// Space Complexity: O(1)
public class Solution {
    public int HammingWeight(uint n) {
        // Use C#'s Convert.ToString method to count bits
        return Convert.ToString(n, 2).Count(c => c == '1');
    }
}
```


