## [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/)

### Approaches
- [Approach 1: Loop and Count](#approach-1-loop-and-count)
- [Approach 2: Brian Kernighan's Algorithm](#approach-2-brian-kernighans-algorithm)
- [Approach 3: Built-in Function](#approach-3-built-in-function)

---

### Approach 1: Loop and Count

**Intuition**

The basic idea is to loop through all 32 bits of the integer and count how many of them are 1s. In each iteration, we can check the last bit of the number using a bitwise AND with 1. Then, we right shift the number by one bit to check the next bit. This approach directly checks each bit, which is straightforward and easy to understand.

**TypeScript Code**

```typescript
function hammingWeight(n: number): number {
    let count = 0;
    
    // Loop through all 32 bits of the integer
    for (let i = 0; i < 32; i++) {
        // Check if the last bit is 1
        if (n & 1) {
            count++;
        }
        // Right shift the number by 1 bit
        n >>= 1;
    }
    
    return count;
}
```

**Complexity Analysis**
- **Time Complexity**: O(1) - The loop always runs for 32 iterations irrespective of the value of `n`.
- **Space Complexity**: O(1) - We only use a constant amount of extra space.

---

### Approach 2: Brian Kernighan's Algorithm

**Intuition**

Instead of checking each bit, we can make use of a property: from the number `n`, `n & (n - 1)` will flip the least significant '1' bit to '0'. By repeatedly performing this operation, we can count how many times we do this until `n` becomes zero. This method leads to fewer iterations because it skips directly to the next '1' bit.

**TypeScript Code**

```typescript
function hammingWeight(n: number): number {
    let count = 0;
    
    // Iterate until n becomes 0
    while (n !== 0) {
        // Flip the least significant 1-bit to 0
        n = n & (n - 1);
        count++;
    }
    
    return count;
}
```

**Complexity Analysis**
- **Time Complexity**: O(k) - where `k` is the number of 1-bits in `n`. This is more efficient than the first approach.
- **Space Complexity**: O(1) - We only use a constant amount of extra space.

---

### Approach 3: Built-in Function

**Intuition**

In some environments, you might have access to a method that can count the number of 1 bits in a binary representation of a number. For instance, languages like JavaScript have `Number.prototype.toString` which converts numbers into strings that include binary options. This isn't necessarily more efficient than Brian Kernighan's logic but can be a quick solution if language features are utilized properly.

**TypeScript Code**

```typescript
function hammingWeight(n: number): number {
    // Convert number to a binary string and split it to check number of '1'
    return n.toString(2).split('0').join('').length;
}
```

**Complexity Analysis**
- **Time Complexity**: O(1) - Assuming string conversion and operations are efficiently handled in constant time.
- **Space Complexity**: O(1) - No extra space beyond the storage of the number itself is required.

Each of these solutions offers a method of counting the 1 bits in a binary number. From checking each bit individually, to leveraging bit manipulation, to underlying language features, there's flexibility in choosing the method that best fits your situation or environment constraints.

