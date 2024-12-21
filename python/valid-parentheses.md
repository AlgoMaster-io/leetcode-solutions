# [Leetcode 20: Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

## Navigation
1. [Approach 1: Brute Force Using String Replacement](#approach-1)
2. [Approach 2: Using Stack](#approach-2)

## Approach 1: Brute Force Using String Replacement

### Intuition:
The idea here is to repeatedly replace occurrences of balanced parentheses pairs ("()", "{}", "[]") with an empty string. This continues until no more replacements are possible. If the final string is empty, all parentheses were valid and balanced.

### Steps:
1. Initialize the string with the input `s`.
2. Iteratively replace "{}", "()", and "[]" with an empty string.
3. Stop when no more replacements occur in a full pass over the string.
4. If the resulting string is empty, return `True`, otherwise return `False`.

### Implementation:
```python
def isValid(s: str) -> bool:
    length = -1
    while length != len(s):
        length = len(s)
        s = s.replace('()', '').replace('[]', '').replace('{}', '')
    return len(s) == 0
```

### Complexity Analysis:
- **Time Complexity**: O(n^2), due to potentially n iterations of string replacements each of which traverses the string.
- **Space Complexity**: O(n), as each replacement creates a new string of reduced size.

## Approach 2: Using Stack

### Intuition:
Use a stack to keep track of opening brackets. When encountering a closing bracket, check the top of the stack for a matching opening bracket.

### Steps:
1. Create an empty stack to hold opening brackets.
2. Traverse through each character in the string `s`.
3. If the character is an opening bracket ('(', '{', '['), push it onto the stack.
4. If a closing bracket is encountered:
   - Check if the stack is not empty and the top of the stack is a matching opening bracket.
   - If true, pop the bracket from the stack; otherwise, return `False`.
5. After processing all characters, if the stack is empty, return `True` (all brackets matched); otherwise, return `False`.

### Implementation:
```python
def isValid(s: str) -> bool:
    stack = []
    matching_brackets = {')': '(', '}': '{', ']': '['}

    for char in s:
        if char in matching_brackets.values():
            stack.append(char)
        elif char in matching_brackets:
            if stack and stack[-1] == matching_brackets[char]:
                stack.pop()
            else:
                return False
        else:
            return False

    return not stack
```

### Complexity Analysis:
- **Time Complexity**: O(n), as we need to traverse through each character in the string once.
- **Space Complexity**: O(n), in the worst case, all opening brackets are pushed onto the stack.

By implementing both approaches, we can handle various input scenarios effectively and choose an optimal solution based on our constraints and requirements.

