# [Leetcode 394: Decode String](https://leetcode.com/problems/decode-string/)

## Approaches:
1. [Recursive Approach](#recursive-approach)
2. [Iterative Stack Approach](#iterative-stack-approach)

---

### Recursive Approach

In this problem, we focus on decoding a string that is encoded in the format k[encoded_string], where the encoded_string inside the square brackets should be repeated k times.

#### Intuition:

1. The idea of the recursive approach is to follow the pattern of the string and evaluate the parts of the string that are recursive themselves.
2. We will maintain a pointer or index to traverse through the string and process each character one by one.
3. When a digit is encountered, it is guaranteed to be the start of an encoded section. We capture the entire number since it can be more than one digit and convert it to an integer `k`.
4. Once we encounter the opening bracket `[`, we recursively decode the substring within brackets.
5. The recursion continues until a closing bracket `]` is encountered, marking the end of the current decoded substring.

#### Code:
```cpp
class Solution {
public:
    // Helper function for the recursive approach
    string decodeStringHelper(const string &s, int &i) {
        string result;

        // Traverse the string sequentially
        while (i < s.length() && s[i] != ']') {
            if (!isdigit(s[i])) {
                // Append regular character to result
                result += s[i++];
            } else {
                // Number found, prepare to decode substring
                int num = 0;
                while (i < s.length() && isdigit(s[i])) {
                    num = num * 10 + (s[i] - '0');
                    i++;
                }
                // Move past the '['
                i++;
                // Recursively decode substring
                string decodedStr = decodeStringHelper(s, i);
                // Multiply the decoded string and append
                while (num-- > 0) result += decodedStr;
                // Move past the ']'
                i++;
            }
        }
        return result;
    }

    // Main function
    string decodeString(string s) {
        int i = 0; // Initialize index for traversing the string
        return decodeStringHelper(s, i);
    }
};
```

#### Complexity Analysis:
- **Time Complexity**: O(n), where n is the length of the input string. Each character is processed once, and recursively the inner strings are processed in a similar manner.
- **Space Complexity**: O(n), space used for the recursion stack.

---

### Iterative Stack Approach

An iterative approach using a stack to maintain current state of the decoding process. We use stacks to keep track of nested encoded strings.

#### Intuition:

1. Use a stack to store the current state: numbers and strings.
2. When encountering a digit, gather the entire number to find the repetition count `k`.
3. When encountering `[`, push the current state (current string and current `k`) onto stacks and reset them.
4. `]` denotes the end of an encoded sequence: pop from the stack, repeat the current sequence, and concatenate with the previous result.
5. Without digits or brackets, append characters to the current string sequence directly.

#### Code:
```cpp
class Solution {
public:
    string decodeString(string s) {
        stack<int> counts;  // Stack to store repeat numbers
        stack<string> resultStack;  // Stack to store results temporarily
        string currentString = "";  // Current work-in-progress decoded string
        int i = 0;

        while (i < s.length()) {
            if (isdigit(s[i])) {
                int n = 0;
                // Form the complete number
                while (i < s.length() && isdigit(s[i])) {
                    n = n * 10 + (s[i] - '0');
                    i++;
                }
                // Save the repeat number
                counts.push(n);
            } else if (s[i] == '[') {
                // Push current string to the stack
                resultStack.push(currentString);
                // Reset current string
                currentString = "";
                i++;
            } else if (s[i] == ']') {
                // Pop previous result
                string temp = resultStack.top();
                resultStack.pop();

                int repeatCount = counts.top();
                counts.pop();

                // Repeat currentString "repeatCount" times and append it to temp
                while (repeatCount-- > 0) temp += currentString;
                
                currentString = temp;  // Save the new decoded string into currentString
                i++; 
            } else {
                // Append character to currentString
                currentString += s[i++];
            }
        }
        return currentString;
    }
};
```

#### Complexity Analysis:
- **Time Complexity**: O(n), efficiency is primarily due to each character being handled exactly once.
- **Space Complexity**: O(n), due to space occupied by stacks.

Both approaches can be implemented according to the nature of recursive or iterative logic preference, balancing ease of implementation and stack management respectively.

