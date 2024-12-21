[LeetCode Problem 227: Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/)

## Table of Contents
- [Approach 1: Using Two Passes (Splitting & Computation)](#approach-1-using-two-passes-splitting-computation)
- [Approach 2: Using Stack for Intermediate Results](#approach-2-using-stack-for-intermediate-results)
- [Approach 3: Optimized One-Pass Solution](#approach-3-optimized-one-pass-solution)

### Approach 1: Using Two Passes (Splitting & Computation)

#### Intuition
The problem is essentially a simple calculator problem. We can solve it by first converting it into an easy-to-use format. The idea is to process the expression by breaking it into numbers and operators. Then, in two passes, compute the result : first for multiplication and division, as they have higher precedence, and then the addition and subtraction.

#### Steps
1. **Handle Spaces and Split**: Clean the expression by removing spaces, then split it into operands and operators.
2. **Compute Multiplication/Division First**: In the first pass, treat any multiplication or division, updating the immediate result.
3. **Compute Addition/Subtraction**: In the second pass, treat the remaining addition and subtraction.

```python
def calculate(s: str) -> int:
    if not s:
        return 0

    # Removing spaces for easy processing
    s = s.replace(" ", "")
    num = 0
    operators_and_numbers = []
    i = 0
    
    while i < len(s):
        if s[i].isdigit():
            num = int(s[i]) + 10 * num
        else:
            operators_and_numbers.append(num)
            operators_and_numbers.append(s[i])
            num = 0
        i += 1
    operators_and_numbers.append(num)
    
    # First pass for multiplication and division
    i = 0
    while i < len(operators_and_numbers):
        if operators_and_numbers[i] == '*' or operators_and_numbers[i] == '/':
            if operators_and_numbers[i] == '*':
                operators_and_numbers[i - 1] *= operators_and_numbers[i + 1]
            elif operators_and_numbers[i] == '/':
                operators_and_numbers[i - 1] //= operators_and_numbers[i + 1]
            operators_and_numbers = operators_and_numbers[:i] + operators_and_numbers[i + 2:]
            i -= 1
        i += 1

    # Second pass for addition and subtraction
    result = operators_and_numbers[0]
    i = 1
    while i < len(operators_and_numbers):
        op = operators_and_numbers[i]
        if op == '+':
            result += operators_and_numbers[i + 1]
        elif op == '-':
            result -= operators_and_numbers[i + 1]
        i += 2
    return result
```
#### Time Complexity: 
- Splitting: O(n)
- Two-pass processing: O(n)
- Total: O(n)

#### Space Complexity: 
- O(n) for list of operators and numbers.


### Approach 2: Using Stack for Intermediate Results

#### Intuition
By using a stack, we can effectively manage operator precedence without requiring a separate pass for different operators. We push intermediate results onto the stack, and for higher precedence operations such as multiplication or division, we pop from the stack, compute, and push the result back.

#### Steps
1. **Iterate through the String**: Traverse each character. If it's a digit, build the number.
2. **Push/Compute Immediate Results**: When an operator is encountered, based on the previous operator:
   - If multiplication or division, compute immediately with the top of the stack.
   - For addition or subtraction, push the number (with sign if necessary).
3. **Sum the Stack**: At the end, the stack will have all remaining operands, and simply summing gives the result.

```python
def calculate(s: str) -> int:
    stack = []
    num = 0
    op = '+'
    i = 0
    
    while i < len(s):
        if s[i].isdigit():
            num = num * 10 + int(s[i])
        
        # When encountering non-digit or end of string, process the previous operators
        if s[i] in '+-*/' or i == len(s) - 1:
            # Process the previous operator
            if op == '+':
                stack.append(num)
            elif op == '-':
                stack.append(-num)
            elif op == '*':
                stack[-1] *= num
            elif op == '/':
                # Force Python's // to truncate towards zero for negative results
                stack[-1] = int(stack[-1] / float(num))
                
            # Reset for the next number
            op = s[i]
            num = 0
        
        i += 1
    
    # The result is the sum of the stack
    return sum(stack)
```

#### Time Complexity: 
- O(n), where n is the length of the string.

#### Space Complexity: 
- O(n) in the worst case for stack size.


### Approach 3: Optimized One-Pass Solution

#### Intuition
To optimize even further, we can utilize variables to track the total and last number and handle operations immediately as they occur without using a stack. This way, we can keep the space requirement constant.

#### Steps
1. **Single Parse with Intermediate Variables**: Use `total`, `current`, and `last_num` to manage the calculations in one simple pass.
2. **Process Operators Immediately**: At each step, update immediate results and adjust `total` based on current computation.

```python
def calculate(s: str) -> int:
    s = s.replace(' ', '')
    total, current, last_num = 0, 0, 0
    op = '+'
    
    i = 0
    while i < len(s):
        if s[i].isdigit():
            current = current * 10 + int(s[i])
        
        if s[i] in '+-*/' or i == len(s) - 1:
            if op == '+':
                total += last_num
                last_num = current
            elif op == '-':
                total += last_num
                last_num = -current
            elif op == '*':
                last_num *= current
            elif op == '/':
                last_num = int(last_num / current)
            
            # Prepare for the new operation
            op = s[i]
            current = 0
        
        i += 1
    
    # Add the final last_num to total
    total += last_num
    return total
```

#### Time Complexity:
- O(n), where n is the length of the input string.

#### Space Complexity:
- O(1), as we only use a fixed number of variables.

Each of these approaches offers a clear way to solve the problem, increasing in complexity with improved efficiency. The choice of method can depend on the ease of implementation or the constraints of the problem.

