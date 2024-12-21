# [Leetcode 6: Zigzag Conversion](https://leetcode.com/problems/zigzag-conversion/)

## Approaches

1. [Simple Row-by-Row Simulation](#approach-1)
2. [Efficient Simulation Using Cycles](#approach-2)

---

## Approach 1: Simple Row-by-Row Simulation

### Intuition:

The idea is to simulate the zigzag pattern by maintaining a list of strings, each representing a row. As we iterate through the characters in the original string, we append them to the appropriate row, zigzagging vertically and diagonally.

### Steps:

1. Create an array of strings for each row.
2. Use a variable to keep track of the current row.
3. Use a boolean to determine the direction of movement (downwards or upwards).
4. Add characters to the current row, and switch direction when the first or last row is reached.

### Time Complexity:

- **O(n)**, where n is the number of characters in the input string `s`.

### Space Complexity:

- **O(n + k)**, where n is for storing the characters and k is the number of rows.

```go
func convert(s string, numRows int) string {
    if numRows == 1 || len(s) <= numRows {
        return s
    }
    
    rows := make([]string, numRows)
    curRow := 0
    goingDown := false

    for _, char := range s {
        // Append current character to the current row
        rows[curRow] += string(char)
        
        // Determine the direction change
        if curRow == 0 || curRow == numRows - 1 {
            goingDown = !goingDown
        }
        // Move up or down based on the direction
        if goingDown {
            curRow++
        } else {
            curRow--
        }
    }

    // Collect all rows into one string
    result := ""
    for _, row := range rows {
        result += row
    }
    
    return result
}
```

---

## Approach 2: Efficient Simulation Using Cycles

### Intuition:

Instead of simulating zigzag row by row, we can recognize that the zigzag traversal forms a repeating cycle across the string. This cycle's length is `2 * numRows - 2`, except for edge cases.

### Steps:

1. Determine the cycle length.
2. Iterate over the string, adding characters that belong to the same row index.
3. Assemble the result by going through each potential row as per the calculated cycle.

### Time Complexity:

- **O(n)**, where n is the number of characters in the input string `s`.

### Space Complexity:

- **O(n)**, for storing the output string.

```go
func convert(s string, numRows int) string {
    if numRows == 1 || len(s) <= numRows {
        return s
    }
    
    rows := make([]string, numRows)
    cycleLen := 2*numRows - 2
    
    for i := 0; i < len(s); i++ {
        // Calculate the index in the cycle for the current character
        cyclePos := i % cycleLen
        
        // Determine the current row
        if cyclePos < numRows {
            rows[cyclePos] += string(s[i])
        } else {
            rows[cycleLen - cyclePos] += string(s[i])
        }
    }

    // Collect all rows into one string
    result := ""
    for _, row := range rows {
        result += row
    }
    
    return result
}
```

Both approaches effectively solve the zigzag conversion problem, but the cycle-based method can be cleaner and slightly more elegant, especially for very large inputs.

