# [Leetcode 155: Min Stack](https://leetcode.com/problems/min-stack/)

## Approaches
1. [Brute Force with Extra Space](#brute-force-with-extra-space)
2. [Optimal Solution with One Stack for Min Values](#optimal-solution-with-one-stack-for-min-values)

### 1. Brute Force with Extra Space

**Intuition**:  
The basic idea is to maintain two separate stacks. One stack (`stack`) keeps track of all elements, while the second stack (`minStack`) maintains the current minimum element in the stack. Whenever we push a new element, we also push the minimum of the current element and the top of the `minStack`. This ensures that `minStack` always has the minimum element at the top.

**Steps**:
- When pushing an element, push it onto the `stack`. In `minStack`, push the minimum of the current element and the current minimum.
- When popping an element, pop from both `stack` and `minStack`.
- The `getMin` operation simply returns the top of `minStack`.

```csharp
public class MinStack 
{
    private Stack<int> stack;
    private Stack<int> minStack;
    
    /** initialize your data structure here. */
    public MinStack() 
    {
        stack = new Stack<int>();
        minStack = new Stack<int>();
    }
    
    public void Push(int x) 
    {
        stack.Push(x);
        // If minStack is empty or x is smaller, push x, else push current min
        if (minStack.Count == 0 || x <= minStack.Peek())
        {
            minStack.Push(x);
        }
        else
        {
            minStack.Push(minStack.Peek());
        }
    }
    
    public void Pop() 
    {
        stack.Pop();
        minStack.Pop();
    }
    
    public int Top() 
    {
        return stack.Peek();
    }
    
    public int GetMin() 
    {
        return minStack.Peek();
    }
}
```

**Complexity Analysis**:
- **Time Complexity**: O(1) for all operations (`Push`, `Pop`, `Top`, and `GetMin`).
- **Space Complexity**: O(n) due to maintaining two stacks for storing all pushed elements and their minimums.

### 2. Optimal Solution with One Stack for Min Values

**Intuition**:  
To optimize space usage, we exploit the fact that stack elements can act as pairs where each pair contains the actual element and the current minimum up to that element. This helps in reducing space by only using one additional stack to keep track of the minimum values at various stages.

**Steps**:
- Use a single stack to store a tuple `(value, currentMin)`.
- On `Push`, determine the new minimum and push `(value, currentMin)`.
- On `Pop`, remove the top value of the stack.
- `GetMin` returns the `currentMin` part of the top tuple.

```csharp
public class MinStack 
{
    private Stack<(int value, int currentMin)> stack;

    public MinStack() 
    {
        stack = new Stack<(int value, int currentMin)>();
    }
    
    public void Push(int x) 
    {
        if (stack.Count == 0)
        {
            stack.Push((x, x));
        }
        else
        {
            int currentMin = Math.Min(stack.Peek().currentMin, x);
            stack.Push((x, currentMin));
        }
    }
    
    public void Pop() 
    {
        stack.Pop();
    }
    
    public int Top() 
    {
        return stack.Peek().value;
    }
    
    public int GetMin() 
    {
        return stack.Peek().currentMin;
    }
}
```

**Complexity Analysis**:
- **Time Complexity**: O(1) for all operations.
- **Space Complexity**: O(n), as we store each element with its minimum at the time of insertion. However, this approach reduces duplication in tracking minimum values.

