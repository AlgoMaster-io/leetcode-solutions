# [Leetcode 6: ZigZag Conversion](https://leetcode.com/problems/zigzag-conversion/)

## Approaches
- [Basic Approach: Simulate Zigzag Pattern](#basic-approach-simulate-zigzag-pattern)
- [Optimal Approach: Direct String Building](#optimal-approach-direct-string-building)

## Basic Approach: Simulate Zigzag Pattern

### Intuition
The basic idea is to simulate filling the characters into rows of the zigzag pattern. We start by initializing an array of empty strings, one for each row. We then iterate over the characters in the string, placing each character into the appropriate row based on the current direction (down or up). After that, we concatenate all rows to form the final string.

### Steps
1. Create an array `rows` with `numRows` empty strings.
2. Use variables to track the current row and the direction of traversal.
3. Traverse each character in the string:
   - Append the character to the current row.
   - If we reach the top/bottom row, change direction.
   - Move to the next row.
4. Concatenate all rows to generate the final result.

### Code

```javascript
function convert(s, numRows) {
    if (numRows === 1 || s.length <= numRows) {
        return s;
    }
    
    const rows = new Array(Math.min(numRows, s.length)).fill('');
    let currentRow = 0;
    let goingDown = false;

    for (let char of s) {
        // Append current character to the current row
        rows[currentRow] += char;
        
        // Change direction if we hit the top or bottom row
        if (currentRow === 0 || currentRow === numRows - 1) {
            goingDown = !goingDown;
        }

        // Move up or down
        currentRow += goingDown ? 1 : -1;
    }
    
    // Concatenate all rows to produce the final output
    return rows.join('');
}
```

### Complexity Analysis
- **Time Complexity**: O(n), where n is the length of the string. We iterate through the string once.
- **Space Complexity**: O(n), for storing the strings for each row.

## Optimal Approach: Direct String Building

### Intuition
In this approach, instead of maintaining multiple rows, we can calculate directly the index where each character of the input string should be placed in the resultant zigzag pattern. We can optimize memory usage by avoiding the temporary storage array completely and building the result string directly.

### Steps
1. Calculate the cycle length as `2 * numRows - 2`.
2. Iterate over each row:
   - Traverse through the string picking the appropriate indices based on the current cycle.
   - Handle the characters in the vertical and diagonal parts specifically.
3. Append the characters to the result string directly.

### Code
```javascript
function convert(s, numRows) {
    if (numRows === 1 || s.length <= numRows) {
        return s;
    }
    
    let result = '';
    const cycleLen = 2 * numRows - 2;

    // Iterate through each row
    for (let i = 0; i < numRows; i++) {
        for (let j = 0; j + i < s.length; j += cycleLen) {
            // Add the character corresponding to the current row and cycle
            result += s[j + i];

            // Handle the diagonal elements in "zigzag" pattern.
            if (i !== 0 && i !== numRows - 1 && j + cycleLen - i < s.length) {
                result += s[j + cycleLen - i];
            }
        }
    }
    
    return result;
}
```

### Complexity Analysis
- **Time Complexity**: O(n), where n is the length of the string. We essentially process each character once.
- **Space Complexity**: O(1), ignoring the space for the input and output strings. We don't use additional data structures proportional to the input size.

