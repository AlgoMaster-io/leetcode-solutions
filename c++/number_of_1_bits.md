# 191. [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/)

## Approach 1: Loop and Bit Manipulation

### Solution
c++
// Time Complexity: O(32) because an integer in C++ is 32 bits.
// Space Complexity: O(1)
```cpp
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int count = 0;
        while (n != 0) {
            // Check if the least significant bit is 1
            count += (n & 1);
            // Right shift to process the next bit
            n >>= 1;
        }
        return count;
    }
};
```

## Approach 2: Brian Kernighan’s Algorithm

### Solution
c++
// Time Complexity: O(1) in the sense that the operation is related to the number of 1s.
// Space Complexity: O(1)
```cpp
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int count = 0;
        while (n != 0) {
            // Drop the lowest set bit
            n &= (n - 1);
            // Increment count for each bit cleared
            count++;
        }
        return count;
    }
};
```

## Approach 3: Using Built-in Function

### Solution
c++
// Time Complexity: O(1) as the function call is executing in a constant time.
// Space Complexity: O(1)
```cpp
class Solution {
public:
    int hammingWeight(uint32_t n) {
        // Use C++'s built-in function to count bits
        return __builtin_popcount(n);
    }
};
```

