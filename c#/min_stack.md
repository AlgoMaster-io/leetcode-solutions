# [155. Min Stack](https://leetcode.com/problems/min-stack/)

## Approach: Two Stacks for Tracking Minimum

### Solution
csharp
```csharp
// Time Complexity:
// - push: O(1)
// - pop: O(1)
// - top: O(1)
// - getMin: O(1)
// Space Complexity: O(n)
using System.Collections.Generic;

public class MinStack {
    private Stack<int> stack;
    private Stack<int> minStack;

    public MinStack() {
        stack = new Stack<int>();
        minStack = new Stack<int>();
    }

    public void Push(int val) {
        stack.Push(val);

        // Push the new minimum onto the minStack
        if (minStack.Count == 0 || val <= minStack.Peek()) {
            minStack.Push(val);
        }
    }

    public void Pop() {
        // If the top element of stack is the current minimum, pop it from minStack too
        if (stack.Peek() == minStack.Peek()) {
            minStack.Pop();
        }
        stack.Pop();
    }

    public int Top() {
        return stack.Peek();
    }

    public int GetMin() {
        return minStack.Peek();
    }
}
```

