# [227. Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/)

## Approach: Using a Stack for Intermediate Results

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
import "unicode"

func calculate(s string) int {
    stack := []int{}
    currentNumber := 0
    operation := '+' // Default operation is addition

    for i, c := range s {
        // Build the current number
        if unicode.IsDigit(c) {
            currentNumber = currentNumber*10 + int(c-'0')
        }

        // Process operators and the last number
        if (!unicode.IsDigit(c) && c != ' ') || i == len(s)-1 {
            switch operation {
            case '+':
                stack = append(stack, currentNumber)
            case '-':
                stack = append(stack, -currentNumber)
            case '*':
                stack[len(stack)-1] *= currentNumber
            case '/':
                stack[len(stack)-1] /= currentNumber
            }

            operation = c // Update the operation
            currentNumber = 0 // Reset the current number
        }
    }

    // Sum up all the values in the stack
    result := 0
    for len(stack) > 0 {
        result += stack[len(stack)-1]
        stack = stack[:len(stack)-1]
    }

    return result
}
```

