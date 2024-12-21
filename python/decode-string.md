# [Leetcode 394: Decode String](https://leetcode.com/problems/decode-string/)

## Approaches
- [Approach 1: Stack based solution](#approach-1:-stack-based-solution)
- [Approach 2: Recursive approach](#approach-2:-recursive-approach)

## Approach 1: Stack based solution

### Intuition
The problem involves nested decoding, and a stack is an ideal data structure for managing nested structures. As we parse the string:

1. Push the characters into a stack.
2. When a closing bracket `]` is encountered, this indicates the end of a segment that needs repetition. We pop from the stack until we reach the matching opening bracket `[`.
3. Collect the digits which represent the number of repetitions for this decoded segment.
4. Decode the segment, append it the specified number of times, and push it back onto the stack.
5. Continue this process until the entire string has been processed.

### Solution
```python
def decodeString(s: str) -> str:
    stack = []  # Initialize the stack

    for char in s:
        if char != ']':
            stack.append(char)  # Push everything except the closing bracket ']'
        else:
            # Start decoding when we see a closing bracket ']'
            decoded_string = []
            while stack[-1] != '[':
                decoded_string.append(stack.pop())  # Collect the string to decode
            decoded_string.reverse()  # Reverse it to get the correct order
            stack.pop()  # Remove the opening bracket '['

            k = []
            while stack and stack[-1].isdigit():
                k.append(stack.pop())  # Gather all the digits for k
            k.reverse()  # Reverse to get the correct number
            k = int(''.join(k))  # Convert list of characters to an integer

            # Repeat the decoded string k times and push it back to the stack
            stack.append(''.join(decoded_string) * k)

    # Join all parts from the stack to form the final decoded string
    return ''.join(stack)
```

### Complexity Analysis
- **Time Complexity:** O(n), where n is the length of the input string `s`. Each character is pushed and popped from the stack once.
- **Space Complexity:** O(n). In the worst case, the space used by the stack is proportional to the size of `s`.

## Approach 2: Recursive approach

### Intuition
The problem has a recursive nature due to nested structures. We can use recursion to decode strings within brackets, repeating segments as specified before returning them to the upper level.

1. A helper function `decode()` is used to parse and decode segments.
2. Every time an opening bracket `[` is encountered, recursion is used to compute the decoded substring.
3. The recursion ends and returns when a closing bracket `]` is reached.
4. Then, decode the current segment based on the repetition count and concatenate the decoded segments in recursion.

### Solution
```python
def decodeString(s: str) -> str:
    def decode(index):
        result = []

        while index < len(s) and s[index] != ']':
            if s[index].isdigit():
                k = 0
                # Form the number for the repeat count
                while index < len(s) and s[index].isdigit():
                    k = k * 10 + int(s[index])
                    index += 1
                # Skip the opening bracket '['
                index += 1
                # Decode the string inside the outermost brackets
                decoded_result, index = decode(index)
                # Append the decoded string k times
                result.append(decoded_result * k)
            else:
                # Append current character if it's a part of the string
                result.append(s[index])
                index += 1

        return ''.join(result), index + 1  # Return concatenated result and index after ']'

    final_result, _ = decode(0)
    return final_result
```

### Complexity Analysis
- **Time Complexity:** O(n), where n is the length of the input string `s`. The function processes each character once.
- **Space Complexity:** O(n). The space consumed in the call stack due to recursion can grow linearly with the depth of the nesting.

