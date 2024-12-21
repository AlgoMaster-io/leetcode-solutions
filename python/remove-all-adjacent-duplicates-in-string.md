# [Leetcode Problem 1047: Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/)

## Approaches
- [Approach 1: Iterative In-Place Modification](#approach-1)
- [Approach 2: Stack Simulation](#approach-2)

## Approach 1: Iterative In-Place Modification

### Intuition
The problem requires us to remove duplicates in a way that no two adjacent letters are the same after each operation. A direct approach is to iterate over the string, and whenever a pair of adjacent duplicates is found, remove them and reset the index to one position before to check for further possible duplicates. This approach emulates a simple iterative check-remove process until no more duplicates can be removed.

### Steps
1. Convert the string into a list for easier in-place modification.
2. Initialize an index to traverse through the list.
3. For each character, if it is the same as the previous, remove both.
4. Adjust the index back to check for further duplicates after each removal.
5. Continue until the end of the list is reached.
6. Convert the list back to a string and return it.

### Code
```python
def removeAdjacentDuplicates(s: str) -> str:
    s = list(s)  # Convert to list for in-place modification
    i = 0  # Initialize pointer to traverse through the list
    while i < len(s) - 1:
        if s[i] == s[i + 1]:
            del s[i:i + 2]  # Remove both duplicates
            if i > 0:
                i -= 1  # Move back one step to check previous possibility
        else:
            i += 1  # Move forward
    return ''.join(s)  # Convert list back to string
```

### Complexity Analysis
- **Time Complexity:** \(O(n^2)\) 
  - In the worst case, we traverse and modify the array repeatedly, most notably when many adjacent duplicates exist that cause frequent removals.
- **Space Complexity:** \(O(n)\)
  - Due to converting the string to a list.

## Approach 2: Stack Simulation

### Intuition
A more optimal way to solve this is using the stack data structure paradigm. The idea is to utilize a stack to store characters and ensure that no two consecutive characters in the stack are the same. If a duplicate is found, we pop the stack and continue processing the next part of the string.

### Steps
1. Initialize an empty stack to hold the resulting characters.
2. Traverse each character in the string.
3. If the stack is not empty and the top of the stack is equal to the current character, pop the stack.
4. Otherwise, push the character to the stack.
5. Join the stack content to form the resultant string.

### Code
```python
def removeAdjacentDuplicates(s: str) -> str:
    result_stack = []  # Initialize an empty stack
    for char in s:
        if result_stack and result_stack[-1] == char:
            result_stack.pop()  # Found a duplicate, so pop
        else:
            result_stack.append(char)  # Push current char to stack
    return ''.join(result_stack)  # Join to form resultant string
```

### Complexity Analysis
- **Time Complexity:** \(O(n)\) 
  - We iterate through the string once while performing constant time operations with the stack.
- **Space Complexity:** \(O(n)\)
  - In worst case, when no duplicates, the stack holds all characters.

This stack-based approach efficiently handles the problem by allowing removing duplicates in a single pass with controlled storage.

