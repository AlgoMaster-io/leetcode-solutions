# [Leetcode 394: Decode String](https://leetcode.com/problems/decode-string/)

### Approaches:
- [Approach 1: Using Recursion](#approach-1-using-recursion)
- [Approach 2: Using Stack](#approach-2-using-stack)

## Approach 1: Using Recursion

### Intuition
The problem of decoding a string can be naturally framed using a recursive approach due to its nested structure. The main challenge is to manage nested levels of encoded strings properly. We define a helper function that processes the string recursively, decoding each level as it goes. By maintaining a pointer, we can decode one layer at a time, descending into deeper layers of nested encoded strings.

```typescript
function decodeStringRecursion(s: string): string {
    let index = 0;  // To keep track of our position in the string

    const decode = (): string => {
        let decodedStr = '';  // Resultant decoded string

        while (index < s.length && s[index] !== ']') {
            if (!isDigit(s[index])) {
                // If current character is a letter, append it to the result
                decodedStr += s[index];
                index++;
            } else {
                // If a digit is encountered, this means start of an encoded sequence
                let k = 0;
                // Construct the multiplier for the sequence
                while (isDigit(s[index])) {
                    k = k * 10 + (s[index].charCodeAt(0) - '0'.charCodeAt(0));
                    index++;
                }
                index++;  // Skip the opening bracket '['
                const decodedPart = decode();  // Decode inside the brackets
                index++;  // Skip the closing bracket ']'
                decodedStr += decodedPart.repeat(k);  // Append the decoded part multiplied by k
            }
        }

        return decodedStr;
    };

    const isDigit = (c: string): boolean => {
        return !isNaN(parseInt(c));
    };

    return decode();
}
```

**Time Complexity:** O(n), where n is the number of characters in the string. In the worst case, each character is processed only a constant number of times.

**Space Complexity:** O(n), due to the call stack in recursion.

## Approach 2: Using Stack

### Intuition
The stack-based approach can effectively simulate recursive processing using an explicit stack. The idea is to push important context into the stack when encountering an opening bracket '[' and pop it back when reaching the corresponding closing bracket ']'. The stack stores pairs of an accumulation of a decoded string up to a point and the multiplier.

```typescript
function decodeStringStack(s: string): string {
    const stack: Array<{currentStr: string, count: number}> = [];
    let currentStr = '';
    let currentNum = 0;

    for (let char of s) {
        if (isDigit(char)) {
            // Build the current number (multiplier)
            currentNum = currentNum * 10 + (char.charCodeAt(0) - '0'.charCodeAt(0));
        } else if (char === '[') {
            // Push the current string and current number onto the stack
            stack.push({currentStr, count: currentNum});
            // Reset current string and currentNumber for new level
            currentStr = '';
            currentNum = 0;
        } else if (char === ']') {
            // Pop from the stack
            const {currentStr: prevStr, count: repeatCount} = stack.pop()!;
            // Decode the current string and concatenate it to the previously built string with repeat count
            currentStr = prevStr + currentStr.repeat(repeatCount);
        } else {
            // Accumulate the character to the current string
            currentStr += char;
        }
    }

    return currentStr;
}

const isDigit = (c: string): boolean => {
    return !isNaN(parseInt(c));
};
```

**Time Complexity:** O(n), as each character is pushed and popped from the stack only once.

**Space Complexity:** O(n), where n is the number of characters in the string—due to stack usage in storing contexts.

