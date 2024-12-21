# [Leetcode 227: Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/)

### Approach 1: Naive Two-Pass, Compute as We Parse

1. [Approach 1: Naive Two-Pass, Compute as We Parse](#approach-1)
2. [Approach 2: Stack for Intermediate Results](#approach-2)
3. [Approach 3: Optimized Single Pass with Immediate Computation](#approach-3)

## Approach 1: Naive Two-Pass, Compute as We Parse

**Intuition:**

This approach involves two passes over the input string. In the first pass, we handle all multiplications and divisions since they have higher precedence, and in the second pass, we evaluate additions and subtractions on the results of the first pass.

**Steps**:
- Split the input by addition and subtraction operators.
- For each resultant component, split further by multiplication and division operators.
- Evaluate the multiplication and division in one pass, then the addition and subtraction in the next pass.

**Code**:

```typescript
function calculate(s: string): number {
    // Remove all spaces from the string
    s = s.replace(/\s+/g, '');

    let numbers = [];
    let num = 0;
    let lastSign = '+';

    for (let i = 0; i < s.length; i++) {
        let char = s[i];

        // Construct the current number
        if (!isNaN(Number(char))) {
            num = num * 10 + parseInt(char);
        }

        if (isNaN(Number(char)) || i === s.length - 1) {
            if (lastSign === '+' || lastSign === '-') {
                // Push the number to list, we deal with `+/-` in another pass
                numbers.push(lastSign === '+' ? num : -num);
            } else if (lastSign === '*') {
                // Perform multiplication
                numbers.push(numbers.pop() * num);
            } else if (lastSign === '/') {
                // Perform integer division
                numbers.push(Math.trunc(numbers.pop() / num));
            }

            // Update the last sign and reset the number
            lastSign = char;
            num = 0;
        }
    }

    // Compute the final sum of the list/stack
    return numbers.reduce((a, b) => a + b, 0);
}
```

**Complexity Analysis**:

- **Time Complexity**: O(n) - We iterate over the string multiple times.
- **Space Complexity**: O(n) - We store numbers temporarily until added up.

## Approach 2: Stack for Intermediate Results

**Intuition:**

This approach uses a stack to handle operations. The key point is that we evaluate multiplication and division immediately, but defer addition and subtraction by storing results in a stack, which we sum up at the end.

**Steps**:
- Traverse through the string while building the current number.
- Use a stack to push numbers resulting from addition and subtraction while immediately handling multiplication and division.
- At the end, sum up all elements in the stack.

**Code**:

```typescript
function calculate(s: string): number {
    s = s.replace(/\s+/g, '');
    let stack: number[] = [];
    let num = 0;
    let lastSign = '+';

    for (let i = 0; i < s.length; i++) {
        let char = s[i];

        if (!isNaN(Number(char))) {
            num = num * 10 + parseInt(char);
        }

        if (isNaN(Number(char)) || i === s.length - 1) {
            if (lastSign === '+') {
                stack.push(num);
            } else if (lastSign === '-') {
                stack.push(-num);
            } else if (lastSign === '*') {
                stack.push(stack.pop() * num);
            } else if (lastSign === '/') {
                stack.push(Math.trunc(stack.pop() / num));
            }

            lastSign = char;
            num = 0;
        }
    }

    return stack.reduce((a, b) => a + b, 0);
}
```

**Complexity Analysis**:

- **Time Complexity**: O(n) - Single traversal of the string.
- **Space Complexity**: O(n) - Stack space to hold intermediate results.

## Approach 3: Optimized Single Pass with Immediate Computation

**Intuition:**

In this improvement, we do not use a stack but rather track the last evaluated number. This allows us to adjust our result dynamically with the current result of multiplication or division, reducing the need for a separate stack.

**Steps**:
- Track `current`, `lastNum`, and `result`.
- As you parse numbers and signs, adjust `lastNum` into the `result` based on whether continuing to add or multiply/divide.

**Code**:

```typescript
function calculate(s: string): number {
    s = s.replace(/\s+/g, '');
    let current = 0;
    let result = 0;
    let lastNum = 0;
    let lastSign = '+';

    for (let i = 0; i < s.length; i++) {
        let char = s[i];

        if (!isNaN(Number(char))) {
            current = current * 10 + parseInt(char);
        }

        if (isNaN(Number(char)) || i === s.length - 1) {
            if (lastSign === '+' || lastSign === '-') {
                // When current comes in, add lastNum to the result
                result += lastNum;
                lastNum = lastSign === '+' ? current : -current;
            } else if (lastSign === '*') {
                lastNum *= current;
            } else if (lastSign === '/') {
                lastNum = Math.trunc(lastNum / current);
            }

            lastSign = char;
            current = 0;
        }
    }

    result += lastNum; // Add the lastNum as we exit the final loop
    return result;
}
```

**Complexity Analysis**:

- **Time Complexity**: O(n) - Only a single pass through the string is needed.
- **Space Complexity**: O(1) - No extra space for stacks or other structures, just variables.

