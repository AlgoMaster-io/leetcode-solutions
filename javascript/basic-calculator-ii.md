# [227. Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/)

## Approaches
- [Approach 1: Brute Force with Stack](#approach-1-brute-force-with-stack)
- [Approach 2: Optimized Stack-based Solution](#approach-2-optimized-stack-based-solution)

### Approach 1: Brute Force with Stack

**Intuition**

In this approach, we'll use a stack to handle the operations. The idea is to parse through the string and evaluate the expression using temporary variables and a stack to process the operations. This method efficiently handles '+' and '-' by using stack additions, and for '*' and '/' by applying them immediately before pushing the result onto the stack.

**Algorithm**:
1. Initialize a stack, a variable `num` to form numbers, and a variable `sign` to track the current sign.
2. Traverse each character:
   - If it's a digit, update `num`.
   - If it's an operator ('+', '-', '*', '/'), handle the previous number with the previous sign, reset `num`, and update `sign`.
   - Finally append any remaining number with its sign.
3. Sum the stack to get the result.

**Time Complexity**: \(O(n)\), where \(n\) is the length of the input string, since we traverse the string once.

**Space Complexity**: \(O(n)\) in the worst case, due to the stack.

```javascript
function calculate(s) {
    const stack = [];
    let num = 0;
    let sign = '+';
    s = s.replace(/\s+/g, ''); // Remove spaces

    for (let i = 0; i < s.length; i++) {
        let char = s[i];
        
        // Construct the number
        if (!isNaN(char)) {
            num = num * 10 + parseInt(char);
        }

        // If the character is an operator or the last character
        if (isNaN(char) || i === s.length - 1) {
            if (sign === '+') {
                stack.push(num);
            } else if (sign === '-') {
                stack.push(-num);
            } else if (sign === '*') {
                stack.push(stack.pop() * num);
            } else if (sign === '/') {
                // Using Math.trunc for integer division
                stack.push(Math.trunc(stack.pop() / num));
            }
            // Update sign and reset number
            sign = char;
            num = 0;
        }
    }
    
    // Sum up the values in the stack as the result
    return stack.reduce((a, b) => a + b, 0);
}
```

### Approach 2: Optimized Stack-based Solution

**Intuition**

An improvement over the first approach is by directly doing the calculation during the parsing in order to avoid any stack use that is not needed. Instead of using a stack to hold all numbers, this method maintains a running total as well as a last computed term for multiplication and division operations.

**Algorithm**:
1. Maintain the same variables as the previous approach.
2. Perform arithmetic as indicated by the operator and update a `currRes`.
3. Multiply or divide the current term immediately, then update results.
4. For '+' and '-', simply add to result at the end.

**Time Complexity**: \(O(n)\), achieving linear time for parsing and computing the result.

**Space Complexity**: \(O(1)\), as no additional space is used beyond variables.

```javascript
function calculate(s) {
    let currRes = 0;    // Current computational state
    let lastNum = 0;    // Last number to include in multiplication/division
    let num = 0;        // Intermediate number parsing
    let sign = '+';     // Last sign seen

    for (let i = 0; i < s.length; i++) {
        let char = s[i];
        
        // Form the current number
        if (!isNaN(char) && char !== ' ') {
            num = num * 10 + parseInt(char);
        }
        
        // If it's an operator or end of string
        if (isNaN(char) || i === s.length - 1) {
            if (sign === '-') {
                currRes -= lastNum;
                lastNum = -num;
            } else if (sign === '+') {
                currRes += lastNum;
                lastNum = num;
            } else if (sign === '*') {
                lastNum *= num;
            } else if (sign === '/') {
                lastNum = Math.trunc(lastNum / num);
            }
            
            sign = char;  // Update the sign for the next number
            num = 0;      // Reset number
        }
    }
    
    // Add the last number segment
    return currRes + lastNum;
}
```

This second approach eliminates the need for a stack by maintaining running sums and keeps the space usage efficient.

