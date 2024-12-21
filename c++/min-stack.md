# [Leetcode 155: Min Stack](https://leetcode.com/problems/min-stack/)

## Approaches:
- [Approach 1: Two Stack Method](#approach-1-two-stack-method)
- [Approach 2: Single Stack with Pair Method](#approach-2-single-stack-with-pair-method)

### Approach 1: Two Stack Method

#### Intuition:
The problem requires us to implement a stack data structure that supports retrieving the minimum element in constant time. A straightforward way to achieve this is by using two stacks: one for storing all values and another for storing the minimum values encountered so far. The second stack ensures that we can obtain the minimum of the stack in constant time, as it tracks the minimum values.

#### Steps:
- Use two stacks: `stack` for holding all the elements of the data structure, and `minStack` for holding the minimums.
- On a `push(x)` operation:
  - Push the element `x` onto the `stack`.
  - If `minStack` is empty or `x` is less than or equal to the current minimum (`minStack.top()`), push `x` onto `minStack` too.
- On a `pop()` operation:
  - Pop an element from `stack`.
  - If the popped element is equal to `minStack.top()`, pop from `minStack` as well.
- `top()` simply returns the top element of `stack`.
- `getMin()` returns the top element of `minStack`, which is always the current minimum of the stack.

#### Code:
```cpp
class MinStack {
private:
    std::stack<int> stack;
    std::stack<int> minStack;

public:
    MinStack() {
        // Initialize the stacks
    }

    void push(int x) {
        stack.push(x);
        if (minStack.empty() || x <= minStack.top()) {
            minStack.push(x);
        }
    }

    void pop() {
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

#### Complexity:
- Time Complexity: O(1) for all operations (`push`, `pop`, `top`, `getMin`) as stack operations are constant time.
- Space Complexity: O(n) where n is the number of elements pushed. In the worst case, the `minStack` could hold all elements if they are in decreasing order.

### Approach 2: Single Stack with Pair Method

#### Intuition:
A more space-efficient solution is to use a single stack but store pairs (value, current minimum) in the stack. This eliminates the need for a separate `minStack`.

#### Steps:
- Use a single stack but push pairs `(value, min)` where `min` is the minimum up to that point.
- On `push(x)`: 
  - Push `(x, min)` onto the stack where `min` is the minimum of `x` and the current minimum (if the stack is not empty).
- On `pop()`, `top()`, and `getMin()`, manage the operations based on the structure of the stored data (access `min` from the top pair for `getMin()`).

#### Code:
```cpp
class MinStack {
private:
    std::stack<std::pair<int, int>> stack;

public:
    MinStack() {
        // Initialize the stack
    }

    void push(int x) {
        if (stack.empty()) {
            stack.push({x, x});
        } else {
            int currentMin = std::min(stack.top().second, x);
            stack.push({x, currentMin});
        }
    }

    void pop() {
        stack.pop();
    }

    int top() {
        return stack.top().first;
    }

    int getMin() {
        return stack.top().second;
    }
};
```

#### Complexity:
- Time Complexity: O(1) for all operations (`push`, `pop`, `top`, `getMin`).
- Space Complexity: O(n), storing pairs, which is more space-efficient overall compared to two stacks in some cases.

In each approach, we ensure that all operations are executed in constant time with varying space trade-offs. The choice of method depends on which space complexity trade-off is acceptable for a given problem scenario.


