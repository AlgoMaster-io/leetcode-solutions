# [Leetcode Problem 227: Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/)

## Approaches
1. [Two-pass Approach with Stack](#two-pass-approach-with-stack)
2. [Single Pass with Variables](#single-pass-with-variables)

### Two-pass Approach with Stack

**Intuition:**
The idea is to use a stack to handle the computation of the arithmetic operations while iterating through the string. This solution handles each number and operator in two passes. In the first pass, we convert the string into numbers and operations and evaluate intermediate results using a stack for final computation in the second pass.

**Steps:**
- Initialize a stack to maintain the intermediary results for operations like addition and subtraction.
- Iterate through the given string to parse numbers, and push them onto the stack.
- For multiplication and division, evaluate immediately and adjust the stack with the latest result.
- Once the iteration completes, sum up all values in the stack to get the final result.

```csharp
public int Calculate(string s) {
    if (string.IsNullOrEmpty(s)) return 0;

    Stack<int> stack = new Stack<int>();
    int currentNumber = 0;
    char operation = '+';

    // Iterate over each character in the string
    for (int i = 0; i < s.Length; i++) {
        char currentChar = s[i];
        
        // Parse numbers fully
        if (Char.IsDigit(currentChar)) {
            currentNumber = currentNumber * 10 + (currentChar - '0');
        }

        // If the current character is an operator, or it's the last character of the string
        if ((!Char.IsDigit(currentChar) && !Char.IsWhiteSpace(currentChar)) || i == s.Length - 1) {
            if (operation == '+') {
                stack.Push(currentNumber);
            } else if (operation == '-') {
                stack.Push(-currentNumber);
            } else if (operation == '*') {
                stack.Push(stack.Pop() * currentNumber);
            } else if (operation == '/') {
                stack.Push(stack.Pop() / currentNumber);
            }
            operation = currentChar;
            currentNumber = 0;
        }
    }

    // Sum up all the numbers in the stack
    int result = 0;
    while (stack.Count != 0) {
        result += stack.Pop();
    }
    return result;
}
```

**Time Complexity:** O(n), where n is the length of the string. We make a single pass over the input string with operations that take constant time per element processed.

**Space Complexity:** O(n), due to the space used by the stack.

### Single Pass with Variables

**Intuition:**
This method optimizes the calculation by maintaining a running total and the result of multiplication/division operations as they are encountered, all in a single iteration without explicitly using a stack. This reduces space complexity by not explicitly storing intermediate results.

**Steps:**
- Use variables to keep track of the current number, the total sum, and the last value for pending operations.
- As you parse each character, accumulate numbers and compute intermediate results for multiplication/division immediately.
- Add or subtract the last operation result from the total when encountering '+' or '-' to handle new numbers.

```csharp
public int Calculate2(string s) {
    if (string.IsNullOrEmpty(s)) return 0;

    int currentNumber = 0;
    int lastNumber = 0;
    int result = 0;
    char operation = '+';

    for (int i = 0; i < s.Length; i++) {
        char currentChar = s[i];

        // Building the number from a series of digits
        if (Char.IsDigit(currentChar)) {
            currentNumber = currentNumber * 10 + (currentChar - '0');
        }

        // Upon encountering an operator or the end, evaluate the expression so far
        if (!Char.IsDigit(currentChar) && !Char.IsWhiteSpace(currentChar) || i == s.Length - 1) {
            if (operation == '+') {
                result += lastNumber; // Add previously computed result
                lastNumber = currentNumber; // Set current number as last pending op result
            } else if (operation == '-') {
                result += lastNumber;
                lastNumber = -currentNumber;
            } else if (operation == '*') {
                lastNumber = lastNumber * currentNumber;
            } else if (operation == '/') {
                lastNumber = lastNumber / currentNumber;
            }
            
            operation = currentChar;
            currentNumber = 0;
        }
    }
    result += lastNumber; // Final addition of last evaluated result
    return result;
}
```

**Time Complexity:** O(n), since we scan the entire string only once.

**Space Complexity:** O(1), as we are using only a fixed amount of extra space.

