# [Leetcode 227: Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/)

## Approaches
- [Approach 1: Basic Stack Simulation](#approach-1)
- [Approach 2: Optimized Single Pass with Variables](#approach-2)

### Approach 1: Basic Stack Simulation

**Intuition:**

This problem is a simulation of evaluating an arithmetic expression consisting of `+`, `-`, `*`, `/`, and non-negative integers. The operations should follow the precedence order where `*` and `/` have a higher precedence than `+` and `-`. The simplest solution to this problem is to utilize a stack to keep track of numbers and handle multiplication and division in place.

**Steps:**
1. Initialize a stack to store numbers.
2. Iterate over the characters:
   - Collect digits to form a number.
   - When encountering an operator or the end of string:
     * Push the current collected number into the stack if it follows a `+` or `-` operator (with continued signs management).
     * Compute immediately for `*` or `/` with the top of the stack.
3. Sum up numbers in the stack to get the result.

```go
func calculate(s string) int {
    // Create a stack of integers
    stack := []int{}
    currentNum := 0
    lastOperator := '+'
    
    // Iterate through the string
    for i := 0; i < len(s); i++ {
        char := s[i]
        
        // If character is a digit build the number
        if isDigit(char) {
            currentNum = currentNum*10 + int(char-'0')
        }
        
        // If character is an operator or the last character
        if !isDigit(char) && !isWhitespace(char) || i == len(s)-1 {
            // Apply the last operator
            switch lastOperator {
            case '+':
                stack = append(stack, currentNum)
            case '-':
                stack = append(stack, -currentNum)
            case '*':
                top := stack[len(stack)-1]
                stack = stack[:len(stack)-1]
                stack = append(stack, top*currentNum)
            case '/':
                top := stack[len(stack)-1]
                stack = stack[:len(stack)-1]
                stack = append(stack, top/currentNum)
            }
            // Update lastOperator and reset currentNum
            lastOperator = rune(char)
            currentNum = 0
        }
    }
    
    // Sum up stack to get the result
    sum := 0
    for _, num := range stack {
        sum += num
    }
    return sum
}

// Helper to check if a character is a digit
func isDigit(ch byte) bool {
    return ch >= '0' && ch <= '9'
}

// Helper to check if a character is whitespace
func isWhitespace(ch byte) bool {
    return ch == ' '
}
```

**Time Complexity:** O(n), where n is the length of the string `s`. We process each character at most twice.

**Space Complexity:** O(n), where n is the length of the string `s`. The worst-case scenario is when we have to store all numbers in the stack.

### Approach 2: Optimized Single Pass with Variables

**Intuition:**

Instead of maintaining a stack to hold numbers and applying operations afterwards, this approach evaluates the operation in real-time. By keeping track of `currentNum`, `lastNum`, and an accumulated `result`, we can compute the resultant value directly as we parse the string.

**Steps:**
1. Traverse the string character by character.
2. Accumulate digits into a number.
3. Determine when to apply an operator: either when hitting an operator or at the end of the string:
   * For the `+` and `-` operators, add or subtract `lastNum` to/from the result.
   * For the `*` and `/` operators, adjust the `lastNum` accordingly and defer adding or subtracting from the result until digging down.
4. Convert and update the `lastNum` and accumulated `result` accordingly as per the arithmetic rules.

```go
func calculateOptimized(s string) int {
    currentNum := 0
    lastNum := 0
    result := 0
    lastOperator := '+'

    for i := 0; i < len(s); i++ {
        char := s[i]
        
        if isDigit(char) {
            currentNum = currentNum*10 + int(char-'0')
        }
        
        if !isDigit(char) && !isWhitespace(char) || i == len(s)-1 {
            switch lastOperator {
            case '+':
                result += lastNum
                lastNum = currentNum
            case '-':
                result += lastNum
                lastNum = -currentNum
            case '*':
                lastNum *= currentNum
            case '/':
                lastNum /= currentNum
            }
            lastOperator = rune(char)
            currentNum = 0
        }
    }
    
    // Add the last processed value 
    result += lastNum
    return result
}
```

**Time Complexity:** O(n), where n is the length of the string `s`. Each character is processed exactly once.

**Space Complexity:** O(1), as no auxiliary stack or large data structure is needed, only constant space for a few variables.

