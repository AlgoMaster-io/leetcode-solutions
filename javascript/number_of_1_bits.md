# 191. [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/)

## Approach 1: Loop and Bit Manipulation

### Solution
```javascript
// Time Complexity: O(32) because a number in JavaScript is considered 32 bits.
// Space Complexity: O(1)
var hammingWeight = function(n) {
    let count = 0;
    while (n !== 0) {
        // Check if the least significant bit is 1
        count += (n & 1);
        // Right shift to process the next bit, use >>> for unsigned shift
        n >>>= 1;
    }
    return count;
};
```

## Approach 2: Brian Kernighan’s Algorithm

### Solution
```javascript
// Time Complexity: O(1) in the sense that the operation is related to the number of 1s.
// Space Complexity: O(1)
var hammingWeight = function(n) {
    let count = 0;
    while (n !== 0) {
        // Drop the lowest set bits
        n &= (n - 1);
        // Increment count for each bit cleared
        count++;
    }
    return count;
};
```

## Approach 3: Using Built-in Function

### Solution
```javascript
// Time Complexity: O(1) as the function call is executing in a constant time.
// Space Complexity: O(1)
var hammingWeight = function(n) {
    // Use JavaScript's built-in method to convert to binary and count bits
    return n.toString(2).split('0').join('').length;
};
```

