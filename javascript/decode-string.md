# [Leetcode 394: Decode String](https://leetcode.com/problems/decode-string/)

## Approaches
- [Approach 1: Stack-based Simulation](#approach-1-stack-based-simulation)
- [Approach 2: Recursive Depth First Search](#approach-2-recursive-depth-first-search)

## Approach 1: Stack-based Simulation

### Intuition
This problem can be approached by simulating the process using a stack to handle the nested structures of the encoded string. 

* Whenever we encounter a digit, we determine the full number that represents how many times the subsequent string section should be repeated.
* For each '[', push the current string and the repeat count onto the stack, reset for the new section.
* When a ']' is encountered, pop from the stack to get the last repeated string and count, build the current substring, then attach it to the string before '['.
* Use a stack to manage the innermost and progressively decode the string outwardly using last-in, first-out structurally intuitive process.

### Solution

```javascript
var decodeString = function(s) {
    let stack = [];
    let currentString = '';
    let currentNumber = 0;
    
    for (let char of s) {
        if (!isNaN(char)) {
            // Form the complete number, as digits can be more than one in length
            currentNumber = currentNumber * 10 + parseInt(char);
        } else if (char === '[') {
            // Push the current state to the stack and start new
            stack.push(currentString);
            stack.push(currentNumber);
            currentString = '';
            currentNumber = 0;
        } else if (char === ']') {
            // Pop the stack to acquire the last number and string,
            // repeat the currentString and append to the previous captured string
            let repeatCount = stack.pop();
            let prevString = stack.pop();
            currentString = prevString + currentString.repeat(repeatCount);
        } else {
            // Accumulate letters in the current section
            currentString += char;
        }
    }
    
    return currentString;
};
```

### Time and Space Complexity
- **Time Complexity**: O(n), where n is the length of the input string. Each character is processed once.
- **Space Complexity**: O(m), where m is the maximum depth of nested structures.

## Approach 2: Recursive Depth First Search

### Intuition
Recursive DFS is useful in parsing deeply nested structures, like this problem. We can keep track of our position in the string and decode sections when '[ ]' are matched.

* As we process characters, construct numbers for repeat counts and substrings.
* Use recursion to dive into subproblems each time '[' is encountered.
* Complete the recursion when ']' is detected, accumulating results and returning them upwards.

### Solution

```javascript
var decodeString = function(s) {
    let i = 0; // Position tracker
    
    function dfs() {
        let currentString = '';
        let currentNumber = 0;
        
        while (i < s.length) {
            const char = s[i];
            if (!isNaN(char)) {
                // Form number for repetition
                currentNumber = currentNumber * 10 + parseInt(char);
                i++;
            } else if (char === '[') {
                // Enter deeper section, move index past '['
                i++;
                const subString = dfs();
                // Append repeated substring result to the current string
                currentString += subString.repeat(currentNumber);
                currentNumber = 0; // Reset current number after use
            } else if (char === ']') {
                // This section complete, return current result
                i++;
                return currentString;
            } else {
                // Accumulate characters as part of the current section
                currentString += char;
                i++;
            }
        }
        
        return currentString;
    }
    
    return dfs();
};
```

### Time and Space Complexity
- **Time Complexity**: O(n), exploring each character in the string just once.
- **Space Complexity**: O(m), where m is the maximum depth of braces due to recursive call stack.

These approaches give clarity to tackling the problem of decoding a string with nested encodings through both iterative stack management and recursive exploration for intuitively clear and efficient solutions.

