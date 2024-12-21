## [Leetcode 394: Decode String](https://leetcode.com/problems/decode-string/)

### Approaches:
1. [Recursive Approach](#recursive-approach)
2. [Iterative Approach using Stack](#iterative-approach-using-stack)

### Recursive Approach

The basic idea to solve this problem is to recursively decode the string following the pattern `k[encoded_string]`, where the integer `k` denotes the number of times the `encoded_string` is repeated. 

Intuition:
- Whenever you encounter a number, it's likely that you're beginning a sequence that will repeat `k` times. This indicates entering a nested condition.
- When you hit `[`, it suggests entering a deeper level, and when `]` is encountered, it's time to finish the current nested condition and return its results to higher levels.
- String concatenation is leveraged to build the resultant decoded string.

```go
func decodeString(s string) string {
    var idx int  // Global index to track string position

    // Helper function for recursive decoding
    var decode func() string
    decode = func() string {
        var res string
        var num int

        for idx < len(s) {
            ch := s[idx]

            if ch >= '0' && ch <= '9' {
                num = num*10 + int(ch-'0')  // Build the number
                idx++
            } else if ch == '[' {
                idx++ // Skip the '['
                decodedString := decode()  // Recursive decode
                for num > 0 {
                    res += decodedString
                    num--
                }
            } else if ch == ']' {
                idx++  // Move past the closing bracket
                break  // Exit current recursive call
            } else {
                res += string(ch)
                idx++
            }
        }
        return res
    }

    return decode()
}
```

**Time Complexity**: O(n), where n is the length of the string. Each character is visited once.

**Space Complexity**: O(n), space used in call stack due to recursion in worst case, when the decoding is very nested.

### Iterative Approach using Stack

In this approach, utilizing a stack can help mimic the call stack of a recursive solution, iteratively storing and managing characters and numbers to build the final decoded string.

Intuition:
- Traverse over each character and manage the encoded information using a stack.
- Numbers define how many times to repeat following substrings once decoded fully.
- Encounters with `[` signal when to push current results into a stack and start with a fresh level.
- Completing the decoding of each bracket sequence (on encountering `]`) leads to pop from the stack and embrace past states with current results.

```go
func decodeString(s string) string {
    numStack := []int{}       // Stack to keep track of numbers (repeat count)
    strStack := []string{}    // Stack to keep track of result strings
    currNum := 0              // Current number being built
    currStr := ""             // Current result string being built

    for _, ch := range s {
        if ch >= '0' && ch <= '9' {
            currNum = currNum*10 + int(ch-'0')  // Build the current iteration number
        } else if ch == '[' {
            numStack = append(numStack, currNum)  // Push the current number
            strStack = append(strStack, currStr)  // Push the current string
            currNum = 0                          // Reset current number
            currStr = ""                         // Reset current string
        } else if ch == ']' {
            repeatCount := numStack[len(numStack)-1]  // Get the last set repeat count
            numStack = numStack[:len(numStack)-1]    // Pop from stack

            decodedStr := strStack[len(strStack)-1] + strings.Repeat(currStr, repeatCount)
            strStack = strStack[:len(strStack)-1]    // Pop string from stack
            currStr = decodedStr                     // Update current string
        } else {
            currStr += string(ch)  // Build the current string
        }
    }
    return currStr
}
```

**Time Complexity**: O(n), as each character in the string is processed at most twice.

**Space Complexity**: O(n), in terms of stack space for storing characters and repetitions.

