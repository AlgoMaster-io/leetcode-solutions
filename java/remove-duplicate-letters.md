# [Leetcode Problem: 316 - Remove Duplicate Letters](https://leetcode.com/problems/remove-duplicate-letters/)

## Approaches:
1. [Greedy approach with Stack and HashMap](#greedy-approach-with-stack-and-hashmap)
2. [Optimized Greedy with Char Array as Stack](#optimized-greedy-with-char-array-as-stack)

---

### Greedy approach with Stack and HashMap

**Intuition:**

The task is to remove duplicate letters and return the lexicographically smallest result. To achieve this, we need to ensure that for each character, we decide whether to keep it or remove it based on its placement and frequency.

#### Steps:
1. Use a HashMap to record the last occurrence of each character in the string. This helps us determine if a character can be safely removed when traversing the string.
2. Use a Stack to build the result string.
3. Use a boolean array to keep track of characters in the current stack to prevent duplicates.
4. Traverse each character in the string:
   - If the character is already in the stack, skip it.
   - Otherwise, compare it with the top of the stack, and if the top character appears later, and the current character is smaller, pop the stack.
   - Push the current character onto the stack and mark it in the boolean array.

5. Convert the stack into the resulting string.

```java
public class Solution {
    public String removeDuplicateLetters(String s) {
        int[] lastOccurrence = new int[26];
        for (int i = 0; i < s.length(); i++) {
            lastOccurrence[s.charAt(i) - 'a'] = i; // last occurrence of every char
        }
        
        boolean[] inStack = new boolean[26]; // to track if the character is in stack to avoid repetition
        Stack<Character> stack = new Stack<>();
        
        for (int i = 0; i < s.length(); i++) {
            char current = s.charAt(i);
            if (inStack[current - 'a']) continue; // if already added in result
            
            // Remove characters from the stack if they appear later and the current character is smaller
            while (!stack.isEmpty() && stack.peek() > current && lastOccurrence[stack.peek() - 'a'] > i) {
                inStack[stack.pop() - 'a'] = false;
            }
            
            stack.push(current);
            inStack[current - 'a'] = true;
        }
        
        StringBuilder result = new StringBuilder();
        for (char c : stack) {
            result.append(c);
        }
        
        return result.toString();
    }
}
```

**Time complexity:** O(n), where n is the length of the string, as we are iterating through the string and each character is pushed and popped from the stack at most once.

**Space complexity:** O(1), since the size of the alphabet is fixed. The stack will at most contain 26 characters.

---

### Optimized Greedy with Char Array as Stack

**Intuition:**

Instead of using a stack data structure, we can use a character array to simulate stack behavior. This slight optimization can lead to performance improvements by reducing method call overheads of Stack operations.

#### Steps:

1. Follow similar logic to the stack-based approach, managing character presence using an array.
2. Use a `char[]` array to act as a stack, managing a pointer to simulate stack-like push/pop operations.

```java
public class Solution {
    public String removeDuplicateLetters(String s) {
        int[] lastOccurrence = new int[26];
        for (int i = 0; i < s.length(); i++) {
            lastOccurrence[s.charAt(i) - 'a'] = i;
        }
        
        boolean[] inArray = new boolean[26];
        char[] result = new char[s.length()]; // Simulate stack with char array
        int stackPointer = 0;
        
        for (int i = 0; i < s.length(); i++) {
            char current = s.charAt(i);
            if (inArray[current - 'a']) continue;
            
            while (stackPointer > 0 && result[stackPointer - 1] > current && lastOccurrence[result[stackPointer - 1] - 'a'] > i) {
                inArray[result[--stackPointer] - 'a'] = false;
            }
            
            result[stackPointer++] = current;
            inArray[current - 'a'] = true;
        }
        
        return new String(result, 0, stackPointer);
    }
}
```

**Time complexity:** O(n), similar to the previous approach.

**Space complexity:** O(1), as we utilize constant additional space in line with the alphabet's size.

---

These approaches leverage the greedy algorithm's principles to ensure that at each step, we make the optimal choice by maintaining the lexicographic ordering while ensuring all characters immediately available are considered. The use of data structures like Stack and an array helps efficiently manage and implement these constraints.

