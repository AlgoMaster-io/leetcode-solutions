# [Leetcode 155: Min Stack](https://leetcode.com/problems/min-stack/)

## Table of Contents
1. [Approach 1: Two Stacks](#approach-1)
2. [Approach 2: Single Stack with Pairs](#approach-2)
3. [Approach 3: Optimized Single Stack](#approach-3)

---

## Approach 1: Two Stacks

### Intuition:
To efficiently get the minimum element, we can maintain two stacks:
1. A primary stack to store all elements that have been pushed.
2. A secondary stack to store the current minimum after each push operation. 

Whenever we perform a `push` operation, we check whether the current element being added is less than the top of the secondary stack, and push this new minimum, otherwise push the current minimum again. This allows `getMin` to be in constant time.

### Implementation:

```javascript
var MinStack = function() {
    this.stack = []; // Primary stack
    this.minStack = []; // Stack to track the minimum
};

/** 
 * @param {number} val
 * @return {void}
 */
MinStack.prototype.push = function(val) {
    this.stack.push(val);
    // Push to minStack the minimum between the new value and the current min
    if (this.minStack.length === 0 || val <= this.minStack[this.minStack.length - 1]) {
        this.minStack.push(val);
    } else {
        this.minStack.push(this.minStack[this.minStack.length - 1]);
    }
};

/**
 * @return {void}
 */
MinStack.prototype.pop = function() {
    this.stack.pop();
    this.minStack.pop();
};

/**
 * @return {number}
 */
MinStack.prototype.top = function() {
    return this.stack[this.stack.length - 1];
};

/**
 * @return {number}
 */
MinStack.prototype.getMin = function() {
    return this.minStack[this.minStack.length - 1];
};
```
- **Time Complexity:** O(1) for all operations: `push`, `pop`, `top`, `getMin`.
- **Space Complexity:** O(n) because we are storing all pushed elements in both the primary and secondary stacks.

---

## Approach 2: Single Stack with Pairs

### Intuition:
Instead of using two different stacks, we can consider storing pairs inside a single stack, where each pair consists of the element and the current minimum value at the time of that element being pushed. This reduces space usage slightly because we don't need an extra stack; we just store more data associated with each element.

### Implementation:

```javascript
var MinStack = function() {
    this.stack = []; // Stack to store (value, minimum) pairs
};

/** 
 * @param {number} val
 * @return {void}
 */
MinStack.prototype.push = function(val) {
    if (this.stack.length === 0) {
        this.stack.push([val, val]); // When stack is empty, both value and minimum are the same
    } else {
        const currentMin = this.stack[this.stack.length - 1][1];
        this.stack.push([val, Math.min(val, currentMin)]);
    }
};

/**
 * @return {void}
 */
MinStack.prototype.pop = function() {
    this.stack.pop();
};

/**
 * @return {number}
 */
MinStack.prototype.top = function() {
    return this.stack[this.stack.length - 1][0];
};

/**
 * @return {number}
 */
MinStack.prototype.getMin = function() {
    return this.stack[this.stack.length - 1][1];
};
```
- **Time Complexity:** O(1) for all operations.
- **Space Complexity:** O(n), since each element stores additional minimum value in the pair.

---

## Approach 3: Optimized Single Stack

### Intuition:
To further optimize, we can maintain a single stack where we store only the current element but manipulate the stack in such a way that we keep track of the minimum. During a pop, we check if we need to update our minimum using clever arithmetic or by using markers.

### Implementation:

This approach generally falls in similar space/time complexity as previous ones with more complex data handling. Approaches mainly differ in clever storage of information to avoid redundancy.

For the regular usage context, approaches 1 or 2 are preferable due to clear and direct handling of values. More complex implementation would involve maintaining minimum result differently to reduce memory. Generally, a transformation on pushing values might be applied so each value in stack has hidden meaning (e.g., storing as offset from minimum).

Due to complexity and varying use cases, we stick with a clearer and straightforward approach as shown previously, which is often enough for competitive programming.

- **Time Complexity:** O(1) for all operations.
- **Space Complexity:** O(n), in practical applications, mitigations might be applied for different handling contexts but usually keep above solutions as base. 

This optimized single stack method often weighs clear understanding and maintainability over minimal theoretical space reductions.

---

These solutions should allow you to efficiently implement a Min Stack with constant time complexity for stack operations surrounded by careful handling of minimum values. The choice among them largely depends on your preferences for clarity versus compactness.

