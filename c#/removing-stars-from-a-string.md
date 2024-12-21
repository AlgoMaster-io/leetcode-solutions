# [Leetcode 2390: Removing Stars From a String](https://leetcode.com/problems/removing-stars-from-a-string/)

## Approaches:

1. [Basic Approach using Stack](#basic-approach-using-stack)
2. [Optimal Approach using Two Pointers](#optimal-approach-using-two-pointers)

---

## Basic Approach using Stack

**Intuition**: The star `'*'` acts as an indicator to remove the preceding character. We can emulate this behavior by using a stack data structure. The idea is to traverse the string and for each character, we either add it to the stack or remove the top character from the stack when a star `'*'` is encountered.

### Steps:
1. Initialize a stack to keep a sequence of characters.
2. Iterate over each character in the string:
   - If the character is `'*'` and the stack is not empty, pop the top element (remove last added character).
   - Otherwise, push the character onto the stack.
3. After traversing the entire string, convert the stack back to a string to get the final result.

```csharp
public class Solution {
    public string RemoveStars(string s) {
        Stack<char> stack = new Stack<char>(); // Initialize the stack

        // Traverse each character in the input string
        foreach (char c in s) {
            if (c == '*') {
                // If current character is '*', remove the top of the stack
                stack.Pop();
            } else {
                // Otherwise, push the current character to the stack
                stack.Push(c);
            }
        }

        // Convert stack contents to string
        char[] result = stack.ToArray();
        Array.Reverse(result); // The stack stores elements in reverse order, so reverse the array
        return new string(result);
    }
}
```

- **Time Complexity**: O(n), where n is the length of the string `s` because we iterate over the string once.
- **Space Complexity**: O(n), in the worst case, all characters are stored in the stack.

---

## Optimal Approach using Two Pointers

**Intuition**: Rather than using a stack, we can use an array to act like a stack and maintain an index to simulate the stack's top pointer. This approach avoids the overhead of stack operations and uses direct array indexing for optimality.

### Steps:
1. Initialize a char array with the same length as the string to store the result in a simulative "stack" manner.
2. Use a pointer to track the current position to insert in this "stack".
3. Iterate through each character in the string:
   - If the character is `'*'`, decrement the insert position (effectively removing the last added character).
   - Otherwise, insert the character at the current position and move the position forward.
4. Finally, convert the relevant portion of the array to a string and return.

```csharp
public class Solution {
    public string RemoveStars(string s) {
        char[] result = new char[s.Length];
        int pos = 0; // This serves as our "stack" pointer

        foreach (char c in s) {
            if (c == '*') {
                // Decrement the insertion pointer to remove the previous character
                pos--;
            } else {
                // Place the character and move the pointer forward
                result[pos] = c;
                pos++;
            }
        }

        // Construct the result string from the array
        return new string(result, 0, pos);
    }
}
```

- **Time Complexity**: O(n), iterating through the string once.
- **Space Complexity**: O(n), due to the usage of an auxiliary array to store the interim result.

The optimal approach reduces overhead from the stack operations by using direct assignments and index manipulations, making it slightly faster and more space-efficient in practice.

