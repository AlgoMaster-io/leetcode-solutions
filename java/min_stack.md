# [155. Min Stack](https://leetcode.com/problems/min-stack/)

## Approach: Two Stacks for Tracking Minimum

### Solution
```java
// Time Complexity:
// - push: O(1)
// - pop: O(1)
// - top: O(1)
// - getMin: O(1)
// Space Complexity: O(n)
import java.util.Stack;

public class MinStack {
    private Stack<Integer> stack;
    private Stack<Integer> minStack;

    public MinStack() {
        stack = new Stack<>();
        minStack = new Stack<>();
    }

    public void push(int val) {
        stack.push(val);

        // Push the new minimum onto the minStack
        if (minStack.isEmpty() || val <= minStack.peek()) {
            minStack.push(val);
        }
    }

    public void pop() {
        // If the top element of stack is the current minimum, pop it from minStack too
        if (stack.peek().equals(minStack.peek())) {
            minStack.pop();
        }
        stack.pop();
    }

    public int top() {
        return stack.peek();
    }

    public int getMin() {
        return minStack.peek();
    }
}
```