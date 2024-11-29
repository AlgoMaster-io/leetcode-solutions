# [155. Min Stack](https://leetcode.com/problems/min-stack/)

## Approach: Two Stacks for Tracking Minimum

### Solution
```python
# Time Complexity:
# - push: O(1)
# - pop: O(1)
# - top: O(1)
# - getMin: O(1)
# Space Complexity: O(n)
class MinStack:
    def __init__(self):
        self.stack = []
        self.minStack = []

    def push(self, val: int) -> None:
        self.stack.append(val)
        # Push the new minimum onto the minStack
        if not self.minStack or val <= self.minStack[-1]:
            self.minStack.append(val)

    def pop(self) -> None:
        # If the top element of stack is the current minimum, pop it from minStack too
        if self.stack[-1] == self.minStack[-1]:
            self.minStack.pop()
        self.stack.pop()

    def top(self) -> int:
        return self.stack[-1]

    def getMin(self) -> int:
        return self.minStack[-1]
```

