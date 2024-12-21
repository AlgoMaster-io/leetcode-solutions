# [Leetcode 1047: Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/)

## Approaches:
1. [Stack-Based Solution](#stack-based-solution)
2. [Optimized Two-Pointer Approach](#optimized-two-pointer-approach)

---

### Stack-Based Solution

#### Intuition:
The problem involves removing adjacent duplicate characters in a string. A stack is well-suited for this task because it can efficiently handle elements in a Last-In-First-Out (LIFO) manner. As we traverse the string, we can use a stack to keep track of characters. If the current character matches the top of the stack, we pop the stack (remove the top element), effectively removing the duplicate. If they don't match, we push the current character onto the stack.

#### Approach:
- Initialize an empty stack.
- Iterate over each character of the string:
  - If the stack is not empty and the top of the stack equals the current character, pop the stack (remove duplicate).
  - Otherwise, push the current character onto the stack.
- Convert the stack to a string which will be the desired result after all operations.

#### Code:
```csharp
public string RemoveDuplicates(string s) {
    Stack<char> stack = new Stack<char>();

    foreach (char c in s) {
        // If the stack is not empty and the top of the stack is equal to the current character
        if (stack.Count > 0 && stack.Peek() == c) {
            stack.Pop(); // Remove the duplicate
        } else {
            stack.Push(c); // Otherwise, push the current character
        }
    }

    // Build the resulting string from the stack
    return new string(stack.Reverse().ToArray());
}
```

#### Time and Space Complexity:
- **Time Complexity:** O(n), where n is the length of the string. We traverse the string once.
- **Space Complexity:** O(n) in the worst case (when no elements are duplicates), as we store all characters in the stack.

---

### Optimized Two-Pointer Approach

#### Intuition:
We can further optimize the space usage from O(n) to O(1) except for the input/output strings by using the input string itself as the "stack". This approach uses a two-pointer technique that simulates a stack directly on the array of characters. We use one pointer to build the result in place and another to iterate through the string.

#### Approach:
- Convert the input string to a character array.
- Use a pointer `i` to track the position of the "top of the stack".
- Iterate over the string with pointer `j`:
  - If `i` is greater than zero and the character at `i - 1` in the array is equal to the current character, decrement `i` (simulating a pop operation).
  - Otherwise, set `arr[i]` to the current character and increment `i` (simulating a push operation).

- Finally, construct the result string from the characters in the character array up to index `i`.

#### Code:
```csharp
public string RemoveDuplicates(string s) {
    char[] arr = s.ToCharArray();
    int i = 0; // This pointer simulates the stack's top

    for (int j = 0; j < s.Length; j++) {
        if (i > 0 && arr[i - 1] == arr[j]) {
            i--; // "pop" operation
        } else {
            arr[i] = arr[j]; // "push" operation
            i++;
        }
    }

    return new string(arr, 0, i); // Construct string from array
}
```

#### Time and Space Complexity:
- **Time Complexity:** O(n), where n is the length of the string, as we process each character once.
- **Space Complexity:** O(n) for the input/output, but no additional space is used beyond the input/output.

This approach reduces the space overhead by modifying the string in place, thereby not relying on additional data structures like a stack.

