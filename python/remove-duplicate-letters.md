## [Leetcode 316: Remove Duplicate Letters](https://leetcode.com/problems/remove-duplicate-letters/)

### Approaches:
- [Approach 1: Greedy Algorithm with Stack](#approach-1-greedy-algorithm-with-stack)

---

### Approach 1: Greedy Algorithm with Stack

**Intuition:**

The problem requires us to generate the smallest lexicographical string after removing duplicate letters while preserving the order. A stack can help to build the result incrementally while maintaining the order and removing duplicates. We will utilize array or hash map to keep track of the last occurrence of each character and an array to ensure the unique constraint.

**Explanation:**

1. **Track Last Occurrence:**
   - First, we record the last occurrence of each character in the string using a dictionary.
   
2. **Stack for Result:**
   - Use a stack to build the final result. This helps in maintaining the traversal order and facilitates easy removal of elements if we find a smaller lexicographical character.
   
3. **Visited Array:**
   - We use an array (or set) to track characters that are already processed (i.e., in the stack).
   
4. **Iterate Over Characters:**
   - For each character:
     - If it is already in the stack, skip it.
     - If the current character is smaller than the last character in the stack and the last character appears later in the string, then remove the last character from the stack to form a possible smaller lexicographical order string.
     - Add the current character to the stack and mark it as visited.

5. **Result Formation:**
   - Join the elements in the stack to form the final result.
   
By following the above steps, we ensure that characters are removed only if a better lexicographical solution exists.

**Complexity Analysis:**

- **Time Complexity:** `O(n)`, where `n` is the length of the string. We make one pass over the string and the operations inside the pass (such as checking stack conditions) are all efficient due to the constraints.
- **Space Complexity:** `O(1)`, as the space needed does not scale with input size but rather depends on the fixed character set size (26 for lowercase English letters).

```python
def removeDuplicateLetters(s: str) -> str:
    # Dictionary to keep track of the last occurrence of each character
    last_occurrence = {c: i for i, c in enumerate(s)}
    
    # Stack to build the result
    stack = []
    # Visited set to track characters already present in stack
    visited = set()
    
    for i, char in enumerate(s):
        # If character is already in stack, skip it
        if char in visited:
            continue
        
        # Conditions to pop from stack:
        # - The stack is not empty and the current character is less than the last character in the stack
        # - The last character in stack appears later in the string as well
        while stack and char < stack[-1] and i < last_occurrence[stack[-1]]:
            # Remove from visited since we're going to pop it
            removed_char = stack.pop()
            visited.remove(removed_char)
        
        # Add current character to stack and mark it visited
        stack.append(char)
        visited.add(char)
    
    # The stack contains the result characters in order
    return ''.join(stack)
```

By considering the above approach, we ensure both an efficient and optimal solution to the problem.

