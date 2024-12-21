# [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/)

## Solutions:
- [Approach 1: Loop through each bit](#approach-1-loop-through-each-bit)
- [Approach 2: Brian Kernighan's Algorithm](#approach-2-brian-kernighans-algorithm)
- [Approach 3: Use Inbuilt Function](#approach-3-use-inbuilt-function)

## Approach 1: Loop through each bit

### Intuition:
The idea here is to evaluate each bit of the number. We can do this by continuously right-shifting the number and checking if the least significant bit (LSB) is set. If it is set (i.e., `bit & 1` equals 1), we increment our count.

### Steps:
1. Initialize a counter to zero.
2. Use a loop to iterate through all 32 bits of the integer.
3. In each iteration, check if the current LSB is 1 using the bitwise AND operation `&`.
4. Shift the bits of the number to the right by 1 to move to the next bit.
5. Return the counter value after examining all bits.

```go
func hammingWeight(num uint32) int {
    count := 0
    for i := 0; i < 32; i++ {
        // Check if the least significant bit is 1
        if num & 1 == 1 {
            count++
        }
        // Shift right to check the next bit
        num >>= 1
    }
    return count
}
```

### Complexity:
- **Time Complexity**: O(32) = O(1), since the loop runs a fixed number of times.
- **Space Complexity**: O(1), no extra space is used except for a few variables.

## Approach 2: Brian Kernighan's Algorithm

### Intuition:
Brian Kernighan's algorithm is a more efficient approach that works by flipping the least significant set bit of the number. Whenever you subtract 1 from a number, all the bits after the rightmost set bit become 1, and that set bit becomes 0. By repeatedly performing `n & (n-1)`, we can count how many set bits there are until the number becomes zero.

### Steps:
1. Initialize a counter to zero.
2. While the number is non-zero, perform `num = num & (num - 1)`.
3. Increment the counter after each operation because it removes one set bit from the number.
4. Return the count when all bits are zero.

```go
func hammingWeight(num uint32) int {
    count := 0
    for num != 0 {
        // Flip the lowest set bit to 0
        num &= (num - 1)
        count++
    }
    return count
}
```

### Complexity:
- **Time Complexity**: O(k), where k is the number of set bits in the number.
- **Space Complexity**: O(1), as we only use a few extra variables.

## Approach 3: Use Inbuilt Function

### Intuition:
Most of the modern programming languages provide built-in functions to count bits. In Go, you can use the standard library function `bits.OnesCount32` from the `math/bits` package.

### Steps:
1. Directly use `bits.OnesCount32(num)` from Go's `math/bits` package.
2. Return the result.

```go
import "math/bits"

func hammingWeight(num uint32) int {
    // Directly using the library function
    return bits.OnesCount32(num)
}
```

### Complexity:
- **Time Complexity**: O(1), as the function is optimized at the language level.
- **Space Complexity**: O(1), no additional space is used.

