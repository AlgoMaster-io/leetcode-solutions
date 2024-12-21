### [Leetcode 394: Decode String](https://leetcode.com/problems/decode-string/)

In this problem, we are required to decode an encoded string where the encoding rule is: k[encoded_string], which means the encoded_string is being repeated exactly k times. The problem presents nested scenarios as well.

In this markdown, we'll discuss various approaches to solve this problem, starting from basic to the most optimal.

### Approaches:
1. [Recursive Approach](#recursive-approach)
2. [Iterative Stack Approach](#iterative-stack-approach)

---

#### Recursive Approach

**Intuition**: 
The encoded strings have a nested nature due to brackets, which makes recursion a natural fit. We process the string and whenever we encounter an opening bracket, it indicates the start of a nested encoded string. We recursively decode this string and then multiply it by the preceding number. The recursion continues until we finish processing the available input.

**Steps**:
1. Use recursion to handle nested encoded strings.
2. Keep track of current position with a global or helper index.
3. When encountering a digit, form the complete multiplier.
4. Upon encountering `[`, recursively process that part.
5. When `]` is encountered, return control to previous recursion level.

**Code**:
```csharp
public class Solution {
    public string DecodeString(string s) {
        int index = 0;
        return Decode(s, ref index);
    }

    private string Decode(string s, ref int index) {
        StringBuilder result = new StringBuilder();
        int number = 0;

        while (index < s.Length) {
            char currentChar = s[index];

            if (Char.IsDigit(currentChar)) {
                // Build the multiplier number.
                number = 10 * number + (currentChar - '0');
            } else if (currentChar == '[') {
                // Start recursion for the nested string.
                index++;
                string decodedString = Decode(s, ref index);
                // Append the decoded string multiplied by the number.
                for (int i = 0; i < number; i++) {
                    result.Append(decodedString);
                }
                number = 0; // Reset the multiplier for the next operation.
            } else if (currentChar == ']') {
                // End of current recursion.
                return result.ToString();
            } else {
                // Accumulate character to the result.
                result.Append(currentChar);
            }
            index++;
        }

        return result.ToString();
    }
}
```

**Time Complexity**: O(n), n is the length of the string (each character could be processed multiple times).

**Space Complexity**: O(n), due to the recursion stack and space needed for the result.

---

#### Iterative Stack Approach

**Intuition**:
The nature of the problem also fits well with a stack-based iterative approach. We utilize stacks to manage the multiplier numbers and the strings extracted thus far. As we parse the string, we push and pop from stacks whenever brackets `[` or `]` are encountered.

**Steps**:
1. Use stacks to track numbers and substrings.
2. When a number is identified, parse it and store.
3. On encountering `[`, push the current state onto stacks and reset state.
4. On `]`, pop the last state, repeat the current substring, and append.
5. Accumulate characters into a live string otherwise.

**Code**:
```csharp
public class Solution {
    public string DecodeString(string s) {
        Stack<int> numStack = new Stack<int>();
        Stack<string> strStack = new Stack<string>();
        StringBuilder currentStr = new StringBuilder();
        int num = 0;

        foreach (char c in s) {
            if (Char.IsDigit(c)) {
                // Formulating the complete number.
                num = num * 10 + (c - '0');
            } else if (c == '[') {
                // Save current state.
                numStack.Push(num);
                strStack.Push(currentStr.ToString());
                // Reset for new sequence.
                num = 0;
                currentStr = new StringBuilder();
            } else if (c == ']') {
                // Decode the current sequence by repeating.
                StringBuilder temp = new StringBuilder(strStack.Pop());
                int repeatTimes = numStack.Pop();
                for (int i = 0; i < repeatTimes; i++) {
                    temp.Append(currentStr);
                }
                currentStr = temp;
            } else {
                // Regular character, append to current output.
                currentStr.Append(c);
            }
        }

        return currentStr.ToString();
    }
}
```

**Time Complexity**: O(n), where n is the length of the string, processing each character.

**Space Complexity**: O(n), due to the stacks and temporary storage of strings.

---

Both methods provide a reliable way to decode strings in the given format. The choice between recursion and iteration can depend on personal preference or specific constraints of the environment, such as stack depth limitations. The stack approach is generally more efficient in environments where recursion depth could become a limiting factor.

