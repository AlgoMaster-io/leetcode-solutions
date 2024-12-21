# [Leetcode 2390: Removing Stars From a String](https://leetcode.com/problems/removing-stars-from-a-string/)

## Approach List
1. [Approach 1: Naive Approach using String Manipulation](#approach-1)
2. [Approach 2: Efficient Approach using Stack](#approach-2)

## Approach 1: Naive Approach using String Manipulation

**Intuition:**

The naive approach to solve this problem involves iterating through the string and using a list to perform insert and delete operations. We can consider each character and maintain a current result string. When we encounter a star ('*'), we'll pop the last character from the result.

**Steps:**

1. Initialize an empty list `res` that will simulate our current result.
2. Iterate over each character in the given string:
   - If the character is '*', pop the last character from the list `res`.
   - Otherwise, append the character to the list `res`.
3. Finally, join the list `res` to form the result string and return it.

**Time Complexity:** O(n)  
**Space Complexity:** O(n)

```python
def removeStars(s: str) -> str:
    res = []
    for char in s:
        if char == '*':
            if res:  # Check if there is a character to remove
                res.pop()  # Remove the last character
        else:
            res.append(char)  # Add the character to the current result
    return ''.join(res)  # Combine list into a string
```

## Approach 2: Efficient Approach using Stack

**Intuition:**

This problem can be efficiently solved using a stack, as stacks provide an ideal last-in, first-out (LIFO) behavior that is useful for handling backspace-like operations represented by '*'. When we encounter a star, we need to remove the last valid character appended.

**Steps:**

1. Use a Python list to simulate a stack.
2. Traverse the input string character by character:
   - If the current character is not '*', push it onto the stack.
   - If the current character is '*', pop the last character from the stack because the star is effectively removing the last valid character.
3. Finally, join all characters in the stack to produce the output string.

**Time Complexity:** O(n)  
**Space Complexity:** O(n)

```python
def removeStars(s: str) -> str:
    stack = []
    for char in s:
        if char == '*':
            if stack:  # Ensure stack is not empty before popping
                stack.pop()  # Remove last character
        else:
            stack.append(char)  # Add character to stack
    return ''.join(stack)  # Create string from characters in stack
```

The efficient stack-based approach effectively handles '*' by removing the last valid character, matching the problem's backspace behavior, and operates efficiently within linear time and space bounds.

