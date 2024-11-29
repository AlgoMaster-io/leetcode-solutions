# [155. Min Stack](https://leetcode.com/problems/min-stack/)

## Approach: Two Stacks for Tracking Minimum

### Solution
```cpp
// Time Complexity:
// - push: O(1)
// - pop: O(1)
// - top: O(1)
// - getMin: O(1)
// Space Complexity: O(n)
#include <stack>
#include <climits>

class MinStack {
private:
    std::stack<int> stack;
    std::stack<int> minStack;

public:
    MinStack() {
        // Nothing to initialize in constructor as stacks are already initialized
    }

    void push(int val) {
        stack.push(val);

        // Push the new minimum onto the minStack
        if (minStack.empty() || val <= minStack.top()) {
            minStack.push(val);
        }
    }

    void pop() {
        // If the top element of stack is the current minimum, pop it from minStack too
        if (stack.top() == minStack.top()) {
            minStack.pop();
        }
        stack.pop();
    }

    int top() {
        return stack.top();
    }

    int getMin() {
        return minStack.top();
    }
};
```

