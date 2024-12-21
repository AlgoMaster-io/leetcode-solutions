# [Leetcode 338: Counting Bits](https://leetcode.com/problems/counting-bits/)

## Approach:
1. [Basic Approach: Using Integer Bit Manipulation](#basic-approach-using-integer-bit-manipulation)
2. [Optimal Approach: Using Dynamic Programming](#optimal-approach-using-dynamic-programming)

---

## Basic Approach: Using Integer Bit Manipulation

### Intuition:
A straightforward method is to count the number of 1s in the binary representation of each number from 0 to n. This can be achieved by iterating through each number and applying the bit manipulation technique. Specifically, for each number, we can repeatedly shift the number right by 1 and check if the least significant bit is 1, counting how many ones we encounter until the number becomes 0.

### Steps:
1. Initialize an array `result` of size `n + 1` to store the count of 1s for each number.
2. For every number `i` from 0 to `n`, count the number of 1s in its binary representation by shifting the bits to the right until the number is 0.
3. Store the count in the `result` array.

### Code:
```csharp
public int[] CountBits(int n) {
    int[] result = new int[n + 1];
    for (int i = 0; i <= n; i++) {
        int count = 0;
        int num = i;
        while (num > 0) {
            // Check if the least significant bit is 1.
            count += num & 1;
            // Shift the number right by 1 to check the next bit.
            num >>= 1;
        }
        result[i] = count;
    }
    return result;
}
```

### Complexity Analysis:
- **Time Complexity**: O(n log n). For each number i, it takes O(log i) to count its bits. Summed over n numbers, this is O(n log n).
- **Space Complexity**: O(1) extra space, as the output array does not count as extra space.

---

## Optimal Approach: Using Dynamic Programming

### Intuition:
This approach leverages the property that the number of 1s in any number `x` can be derived from the result of `x >> 1`, plus 1 if the last bit of x is 1. This is because right shifting effectively divides a number by 2, and the remainder (0 or 1, which we access using `x & 1`) determines if there's an additional 1 in the binary representation.

### Steps:
1. Initialize an array `result` of size `n + 1` to store the count of 1s for each number, starting with `result[0] = 0`.
2. For each number `i` from 1 to `n`, calculate the number of 1s by using the formula `result[i] = result[i >> 1] + (i & 1)`.
3. Return the `result` array.

### Code:
```csharp
public int[] CountBits(int n) {
    int[] result = new int[n + 1];
    // Start with the base case
    result[0] = 0;
    
    for (int i = 1; i <= n; i++) {
        // result[i >> 1] gives the count for i / 2, and i & 1 adds 1 if i is odd.
        result[i] = result[i >> 1] + (i & 1);
    }
    
    return result;
}
```

### Complexity Analysis:
- **Time Complexity**: O(n). We fill each entry of the result array once.
- **Space Complexity**: O(1) extra space, the output array is excluded.

---

These solutions provide a comprehensive understanding from a basic brute force approach to an optimal dynamic programming technique to solve the problem efficiently.

