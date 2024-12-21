[Leetcode Problem 20: Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

### Approaches:
- [Approach 1: Stack Data Structure](#approach-1-stack-data-structure)
- [Approach 2: Optimized Stack with Early Exit](#approach-2-optimized-stack-with-early-exit)

### Approach 1: Stack Data Structure

#### Intuition:
This problem is classic for using a stack data structure which follows the Last-In-First-Out (LIFO) principle. The main idea here is to push each opening bracket encountered onto the stack and pop it when a corresponding closing bracket is encountered. If at the end the stack is empty, it means that all the parentheses were correctly matched.

#### Steps:
1. Initialize an empty stack.
2. Traverse the string character by character.
3. For every opening bracket (`'('`, `'{'`, `'['`), push it onto the stack.
4. For every closing bracket (`')'`, `'}'`, `']'`), check if the stack is empty (which means there's no corresponding opening), and if not, pop from the stack and check if it matches the current character.
5. After all characters have been processed, ensure the stack is empty to guarantee all opening brackets were properly closed.

#### Time Complexity:
- O(n), where n is the length of the string; we traverse each character once.

#### Space Complexity:
- O(n), in the worst case for the stack if all characters are opening brackets.

```csharp
public bool IsValid(string s) {
    Stack<char> stack = new Stack<char>();
    
    // Dictionary to hold matching pairs
    Dictionary<char, char> mapping = new Dictionary<char, char>();
    mapping.Add(')', '(');
    mapping.Add('}', '{');
    mapping.Add(']', '[');

    foreach (char ch in s) {
        // If it is a closing bracket
        if (mapping.ContainsKey(ch)) {
            // Pop from stack or use a dummy value if stack is empty
            char topElement = stack.Count == 0 ? '#' : stack.Pop();

            // Check if the popped element matches the opening bracket
            if (topElement != mapping[ch]) {
                return false;
            }
        } else {
            // It's an opening bracket, push onto the stack
            stack.Push(ch);
        }
    }

    // If the stack is empty, all the parentheses were properly closed
    return stack.Count == 0;
}
```

---

### Approach 2: Optimized Stack with Early Exit

#### Intuition:
An optimization can be achieved in cases where the number of characters is odd because a valid sequence of parentheses always has an even number of characters. We can terminate early if an imbalance is detected or if there are more closing brackets than opening brackets during the iteration which means it can never be a valid sequence.

#### Steps:
1. Check if the length of the string is odd. Return false immediately.
2. Initialize an empty stack.
3. Traverse the string similar to the first approach and check immediately for mismatches—if a mismatch is found, return false early.
4. Return false if the stack is not empty by the end of iteration as there would be unmatched opening brackets left.

#### Time Complexity:
- Still O(n) due to iterating through the string, but can terminate early in some cases.

#### Space Complexity:
- O(n), remains the same due to stack storage.

```csharp
public bool IsValid(string s) {
    // Immediately fail if string length is odd
    if (s.Length % 2 != 0) return false;

    Stack<char> stack = new Stack<char>();
    Dictionary<char, char> mapping = new Dictionary<char, char> {
        { ')', '(' },
        { '}', '{' },
        { ']', '[' }
    };

    foreach (char ch in s) {
        if (mapping.ContainsKey(ch)) {
            char topElement = stack.Count == 0 ? '#' : stack.Pop();
            if (topElement != mapping[ch]) {
                return false;
            }
        } else {
            stack.Push(ch);
        }
    }

    return stack.Count == 0;
}
```

In both approaches, handling of the stack ensures the check is robust for a correct validation of parentheses, and this solution is efficient for this type of problem.

