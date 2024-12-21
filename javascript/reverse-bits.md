### Problem: [Leetcode 190: Reverse Bits](https://leetcode.com/problems/reverse-bits/)

In this problem, you are given a 32-bit unsigned integer and you need to reverse the bits of this integer.

### Approaches

1. [Basic Approach: Naive Bit Manipulation](#basic-approach-naive-bit-manipulation)
2. [Optimal Approach: Bit Manipulation with Shift Operations](#optimal-approach-bit-manipulation-with-shift-operations)

---

### Basic Approach: Naive Bit Manipulation

**Intuition:**

We can solve this problem by processing each bit from the input number one by one, and flipping them into the reverse position of a new number as we iterate through the original number.

**Detailed Steps:**

1. Initialize a new variable `reversed` with 0.
2. Iterate 32 times (since we are dealing with 32 bits).
3. For each bit:
   - Left-shift `reversed` by 1 bit to make space for the new bit.
   - Extract the last bit of `n` and add it to `reversed`.
   - Right-shift `n` by 1 to process the next bit.
4. After 32 iterations, `reversed` will contain the reversed bits of the original number.

**Code:**

```javascript
var reverseBits = function(n) {
    let reversed = 0;
    
    // Iterate through all 32 bits of the input number
    for (let i = 0; i < 32; i++) {
        // Left-shift 'reversed' to make space for the new bit
        reversed <<= 1;
        
        // Add the last bit of 'n' to 'reversed'
        reversed |= (n & 1);
        
        // Right-shift 'n' to bring the next bit to the LSB position
        n >>= 1;
    }
    
    return reversed >>> 0; // Convert to unsigned 32-bit integer before returning
};
```

**Time Complexity:** O(1) - The loop runs a fixed number of 32 times.

**Space Complexity:** O(1) - We are using a constant amount of space.

---

### Optimal Approach: Bit Manipulation with Shift Operations

**Intuition:**

This approach is similar to the basic one but focuses more on using fewer operations by combining left shifts and OR operations more efficiently.

**Detailed Steps:**

1. The overall logic remains similar; however, by processing bits in chunks rather than individually, we can potentially improve performance.
2. Initialize a new variable `reversed` to store the result.
3. Iterate through each bit, but envision storing results more efficiently using compact shifts.
4. Advance both reversal process and input number transformation more smartly with shifts.

**Code Explanation:** Given the nature of the problem, the above solution is already optimal for JavaScript as shifting operations beyond this add complexity without additional advantage due to fixed loop iterations.

**Code Remains Same:**

```javascript
var reverseBits = function(n) {
    let reversed = 0;

    for (let i = 0; i < 32; i++) {
        reversed <<= 1;
        reversed |= (n & 1);
        n >>= 1;
    }

    return reversed >>> 0;
};
```

**Time Complexity:** O(1) - Always runs a fixed number of operations regardless of input.

**Space Complexity:** O(1) - Uses a fixed amount of memory.

---

This problem highlights fundamental bit manipulation skills essential for understanding low-level operations in computer science.

