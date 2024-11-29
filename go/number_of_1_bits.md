# 191. [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/)

## Approach 1: Loop and Bit Manipulation

### Solution
go
```go
// Time Complexity: O(32) because an integer in Go is 32 bits.
// Space Complexity: O(1)
package main

func hammingWeight(num uint32) int {
    count := 0
    for num != 0 {
        // Check if the least significant bit is 1
        count += int(num & 1)
        // Right shift to process the next bit
        num >>= 1
    }
    return count
}
```

## Approach 2: Brian Kernighan’s Algorithm

### Solution
go
```go
// Time Complexity: O(1) in the sense that the operation is related to the number of 1s.
// Space Complexity: O(1)
package main

func hammingWeight(num uint32) int {
    count := 0
    for num != 0 {
        // Drop the lowest set bit
        num &= num - 1
        // Increment count for each bit cleared
        count++
    }
    return count
}
```

## Approach 3: Using Built-in Function

### Solution
go
```go
// Time Complexity: O(1) as the function call is executing in a constant time.
// Space Complexity: O(1)
package main

import "math/bits"

func hammingWeight(num uint32) int {
    // Use Go's bits package to count bits
    return bits.OnesCount32(num)
}
```

