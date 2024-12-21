[Leetcode Problem 155: Min Stack](https://leetcode.com/problems/min-stack/)

## Approaches
1. [Basic Approach: Two Stacks](#approach-1)
2. [Optimal Approach: One Stack with Pair](#approach-2)

### Approach 1: Two Stacks

#### Intuition:
The basic idea for creating a min stack is to use two stacks: one stack to store the normal values and another stack to keep track of the minimum values. The second stack, `min_stack`, helps in keeping track of the minimum element up to the current point in `stack_main`.

- **Push Operation**: When an element is pushed onto `stack_main`, it's also pushed onto `min_stack` if it is the new minimum.
- **Pop Operation**: When an element is popped from `stack_main`, the top element from `min_stack` is also popped if it is equal to the element being popped.
- **Top Operation**: Simply return the top of `stack_main`.
- **GetMin Operation**: Retrieve the top of `min_stack`.

#### Python Code:
```python
class MinStack:

    def __init__(self):
        # Main stack to store all values
        self.stack_main = []
        # Auxiliary stack to store current minimum
        self.min_stack = []

    def push(self, val: int) -> None:
        # Push the value onto the main stack
        self.stack_main.append(val)
        
        # If the min_stack is empty or the current value is <= to the top of min_stack, push it onto min_stack
        if not self.min_stack or val <= self.min_stack[-1]:
            self.min_stack.append(val)

    def pop(self) -> None:
        # Pop from the main stack
        top_val = self.stack_main.pop()
        
        # If the popped value is the same as the top of min_stack, pop from min_stack
        if top_val == self.min_stack[-1]:
            self.min_stack.pop()

    def top(self) -> int:
        # Return the top element of main stack
        return self.stack_main[-1]

    def getMin(self) -> int:
        # Return the top element of min stack (minimum element)
        return self.min_stack[-1]
```

#### Time Complexity:
- **push**: O(1)
- **pop**: O(1)
- **top**: O(1)
- **getMin**: O(1)

#### Space Complexity:
- O(n) for storing all elements in two stacks.

### Approach 2: One Stack with Pair

#### Intuition:
In this approach, instead of using two separate stacks, we can use a single stack to store pairs of `(value, current_min)`. Every element pushed compares its value with the current minimum stored in the stack and pushes itself along with the most current minimum.

- **Push Operation**: When a new element arrives, we compute the new minimum by comparing the element with the current minimum (if the stack is not empty).
- **Pop Operation**: Simply pop the top element (a pair) of the stack.
- **Top Operation**: Return the value part of the top element of the stack.
- **GetMin Operation**: Return the current minimum part from the top element of the stack.

#### Python Code:
```python
class MinStack:

    def __init__(self):
        # Stack to store tuples of (value, current_min)
        self.stack = []

    def push(self, val: int) -> None:
        if not self.stack:
            # If stack is empty, the new value is also the current minimum
            current_min = val
        else:
            # Current min is the lesser of the new value or the current min
            current_min = min(val, self.stack[-1][1])
        # Push the new value along with the current minimum onto the stack
        self.stack.append((val, current_min))

    def pop(self) -> None:
        # Pop the top element, which is a tuple of (value, current_min)
        self.stack.pop()

    def top(self) -> int:
        # Return the value part of the top element's tuple (value, current_min)
        return self.stack[-1][0]

    def getMin(self) -> int:
        # Return the current min part of the top element's tuple (value, current_min)
        return self.stack[-1][1]
```

#### Time Complexity:
- **push**: O(1)
- **pop**: O(1)
- **top**: O(1)
- **getMin**: O(1)

#### Space Complexity:
- O(n) for storing elements as tuples in a single stack.

Both approaches efficiently manage operations to keep track of the minimum value in constant time, while the second approach uses just a single stack, keeping the solution cleaner and more efficient space-wise.

