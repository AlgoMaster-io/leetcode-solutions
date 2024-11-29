# [227. Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/)

## Approach: Using a Stack for Intermediate Results

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(n)

class Solution:
    def calculate(self, s: str) -> int:
        stack = []
        current_number = 0
        operation = '+'  # Default operation is addition

        for i in range(len(s)):
            c = s[i]

            # Build the current number
            if c.isdigit():
                current_number = current_number * 10 + int(c)

            # Process operators and the last number
            if not c.isdigit() and c != ' ' or i == len(s) - 1:
                if operation == '+':
                    stack.append(current_number)
                elif operation == '-':
                    stack.append(-current_number)
                elif operation == '*':
                    stack.append(stack.pop() * current_number)
                elif operation == '/':
                    stack.append(int(stack.pop() / current_number))  # Cast to int for truncation towards zero

                operation = c  # Update the operation
                current_number = 0  # Reset the current number

        # Sum up all the values in the stack
        return sum(stack)
```

