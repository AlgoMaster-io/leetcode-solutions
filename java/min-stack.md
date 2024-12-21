# [155. Min Stack](https://leetcode.com/problems/min-stack/)

## Approaches:
1. [Basic Solution: Using Two Stacks](#basic-solution-using-two-stacks)
2. [Optimized Solution: Using Single Stack with Pair Value](#optimized-solution-using-single-stack-with-pair-value)
3. [Optimized Solution: Single Stack with Min Tracking](#optimized-solution-single-stack-with-min-tracking)

### Basic Solution: Using Two Stacks

#### Intuition:
The idea here is to use two stacks. One stack will be used as a regular stack to store all the numbers, whereas the second stack (let's call it the minStack) will be used to keep track of the minimum value at each stage. Whenever we push an element, we'll also push the new minimum onto the minStack. When we pop an element, we'll also pop from the minStack to ensure both stacks stay synchronized. 

#### Code:
```java
class MinStack {
    private Stack<Integer> stack;
    private Stack<Integer> minStack;

    /** initialize your data structure here. */
    public MinStack() {
        stack = new Stack<>();
        minStack = new Stack<>();
    }

    public void push(int val) {
        // Push the value to the stack
        stack.push(val);
        // Push the minimum value seen so far into the minStack
        if (minStack.isEmpty() || val <= minStack.peek()) {
            minStack.push(val);
        } else {
            minStack.push(minStack.peek());
        }
    }

    public void pop() {
        // Pop from both stack and minStack
        stack.pop();
        minStack.pop();
    }

    public int top() {
        // Return the top element
        return stack.peek();
    }

    public int getMin() {
        // Return the top of minStack, which is the minimum
        return minStack.peek();
    }
}
```

#### Complexity Analysis:
- **Time Complexity**: O(1) for each operation (`push`, `pop`, `top`, and `getMin`).
- **Space Complexity**: O(n) where n is the number of elements in the stack, since we are using two stacks.

### Optimized Solution: Using Single Stack with Pair Value

#### Intuition:
Rather than using two separate stacks, we can optimize space by using a single stack, where each element in the stack is a pair consisting of the actual value and the minimum value up to that point.

#### Code:
```java
class MinStack {
    private Stack<int[]> stack;

    /** initialize your data structure here. */
    public MinStack() {
        stack = new Stack<>();
    }

    public void push(int val) {
        // If the stack is empty, the minimum is the value itself
        if (stack.isEmpty()) {
            stack.push(new int[]{val, val});
        } else {
            // Otherwise, push a pair of val and minimum of val and stack's current minimum
            int currentMin = Math.min(val, stack.peek()[1]);
            stack.push(new int[]{val, currentMin});
        }
    }

    public void pop() {
        stack.pop();
    }

    public int top() {
        return stack.peek()[0];
    }

    public int getMin() {
        return stack.peek()[1];
    }
}
```

#### Complexity Analysis:
- **Time Complexity**: O(1) for each operation.
- **Space Complexity**: O(n), but now we use only a single stack.

### Optimized Solution: Single Stack with Min Tracking

#### Intuition:
Another approach is to keep track of the minimum value in a variable. We push new values onto the stack, and if a new value is the new minimum, we first push the previous minimum onto the stack and then push the new value. The main advantage of this approach is that we use a single stack and minimum variables to efficiently track the minimum value.

#### Code:
```java
class MinStack {
    private Stack<Integer> stack;
    private int min;

    /** initialize your data structure here. */
    public MinStack() {
        stack = new Stack<>();
    }

    public void push(int val) {
        // If the stack is empty, set min to the val
        if (stack.isEmpty()) {
            min = val;
        } else if (val <= min) {
            // Push current minimum to the stack before the new min
            stack.push(min);
            min = val;
        }
        // Push the value to stack
        stack.push(val);
    }

    public void pop() {
        // If we pop the min value, pop another element to update min
        if (stack.pop() == min && !stack.isEmpty()) {
            min = stack.pop();
        }
    }

    public int top() {
        return stack.peek();
    }

    public int getMin() {
        return min;
    }
}
```

#### Complexity Analysis:
- **Time Complexity**: O(1) for each operation.
- **Space Complexity**: O(n), but more efficient stack utilization by storing the minimum only when necessary.

