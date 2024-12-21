[Leetcode 155: Min Stack](https://leetcode.com/problems/min-stack/)

Approaches:
- [Approach 1: Two independent stacks (Basic Approach)](#approach-1-two-independent-stacks-basic-approach)
- [Approach 2: Single stack with tuple values (Optimized approach)](#approach-2-single-stack-with-tuple-values-optimized-approach)
- [Approach 3: Single stack with min maintenance (Most optimal)](#approach-3-single-stack-with-min-maintenance-most-optimal)

### Approach 1: Two independent stacks (Basic Approach)

**Intuition**:
The simplest way to tackle the Min Stack problem is by using two separate stacks:
1. The main stack to store all the elements.
2. The secondary stack to keep track of the minimum value at each level of the main stack.

Every time we push an element, we also push it into the min stack if it is less than or equal to the current minimum. During a pop operation, if the popped element is the same as the top of the min stack, we also pop from the min stack.

**Code**:
```typescript
class MinStack {
    private stack: number[];
    private minStack: number[];

    constructor() {
        this.stack = [];
        this.minStack = [];
    }

    push(val: number): void {
        // Push onto the main stack
        this.stack.push(val);
        // If the min stack is empty or this value is <= current min, push it to the min stack
        if (this.minStack.length === 0 || val <= this.minStack[this.minStack.length - 1]) {
            this.minStack.push(val);
        }
    }

    pop(): void {
        const poppedValue = this.stack.pop();
        // If the popped value is the same as the top of the min stack, pop the min stack
        if (poppedValue === this.minStack[this.minStack.length - 1]) {
            this.minStack.pop();
        }
    }

    top(): number {
        // Return the top of the main stack
        return this.stack[this.stack.length - 1];
    }

    getMin(): number {
        // The current min stack top is the minimum element
        return this.minStack[this.minStack.length - 1];
    }
}
```

**Time Complexity**: 
- `push()`, `pop()`, `top()`, `getMin()` all are O(1) because stack operations (push, pop, and top) are O(1).

**Space Complexity**: 
- O(n) in the worst case, as we might store all elements in both stacks.

### Approach 2: Single stack with tuple values (Optimized approach)

**Intuition**:
Another efficient way is to use a single stack, where each element is a tuple consisting of the value and the current minimum. This way, we don't need a separate stack for the minimum values.

**Code**:
```typescript
class MinStack {
    private stack: Array<{ value: number, min: number }>;

    constructor() {
        this.stack = [];
    }

    push(val: number): void {
        // Calculate the new minimum
        const currentMin = this.stack.length === 0 ? val : this.getMin();
        const min = Math.min(val, currentMin);
        // Push a tuple (value, current min)
        this.stack.push({ value: val, min: min });
    }

    pop(): void {
        // Remove the top element
        this.stack.pop();
    }

    top(): number {
        // Return the top value
        return this.stack[this.stack.length - 1].value;
    }

    getMin(): number {
        // Return the min associated with the top element
        return this.stack[this.stack.length - 1].min;
    }
}
```

**Time Complexity**: 
- `push()`, `pop()`, `top()`, `getMin()` all are O(1).

**Space Complexity**: 
- O(n) as we're maintaining a single stack with extra storage for the min value.

### Approach 3: Single stack with min maintenance (Most optimal)

**Intuition**:
With a single stack approach, we can utilize a simple trick of storing the difference between each value and its current min. This is a very space-efficient method which stores the stack’s elements while keeping track of the minimum by leveraging only numerical differences.

**Code**:
```typescript
class MinStack {
    private stack: number[];
    private min: number;

    constructor() {
        this.stack = [];
        this.min = Infinity;
    }

    push(val: number): void {
        if (this.stack.length === 0) {
            this.stack.push(0);
            this.min = val;
        } else {
            // Store the difference between the new value and current min
            this.stack.push(val - this.min);
            // Update the min if the new value is less
            if (val < this.min) {
                this.min = val;
            }
        }
    }

    pop(): void {
        if (this.stack.length === 0) return;

        const pop = this.stack.pop();
        // If the popped value is less than 0, it signals that it was a minimum. Restore previous min.
        if (pop < 0) {
            this.min = this.min - pop;
        }
    }

    top(): number {
        if (this.stack.length === 0) throw "Stack is empty!";

        const topValue = this.stack[this.stack.length - 1];
        // Return the calculated correct top value
        if (topValue > 0) {
            return topValue + this.min;
        } else {
            return this.min;
        }
    }

    getMin(): number {
        return this.min;
    }
}
```

**Time Complexity**: 
- `push()`, `pop()`, `top()`, `getMin()` all are O(1).

**Space Complexity**: 
- O(1) for the min storage enhancement. The stack itself still grows with the number of elements (O(n)) but no extra data structures for min tracking.

