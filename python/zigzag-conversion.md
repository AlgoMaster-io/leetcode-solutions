# [Leetcode 6: Zigzag Conversion](https://leetcode.com/problems/zigzag-conversion/)

## Approaches:
1. [Basic Approach: Simulate Zigzag Pattern](#basic-approach-simulate-zigzag-pattern)
2. [Optimized Using Cycle Calculation](#optimized-using-cycle-calculation)

---

## Basic Approach: Simulate Zigzag Pattern

### Intuition:
The basic idea is to simulate the process of writing characters in a zigzag pattern row by row. We move downwards to fill each row and once we hit the bottom row, we change direction to move upwards diagonally.

### Steps:
1. Create an array where each element is a string corresponding to a row in the zigzag pattern.
2. Use a variable to keep track of the current row and a flag to indicate the current direction (up or down).
3. Traverse the input string character by character, placing each character in the correct row.
4. Adjust direction when the top or bottom row is reached.
5. Concatenate all the rows for the final result.

### Python Code:
```python
def convert(s: str, numRows: int) -> str:
    # Edge case: If only one row, return the string itself.
    if numRows == 1 or numRows >= len(s):
        return s

    # List of strings, each represents a row in the zigzag pattern.
    rows = [''] * numRows
    current_row, step = 0, 1

    # Loop through each character in the string
    for char in s:
        # Add the character to the current row
        rows[current_row] += char
        
        # Decide whether we need to move up or down
        if current_row == 0:
            step = 1
        elif current_row == numRows - 1:
            step = -1
        
        # Move to the next row
        current_row += step

    # Concatenate all rows to get the final converted string
    return ''.join(rows)

```

### Complexity Analysis:
- **Time Complexity:** O(n), where n is the length of the string `s`, as we iterate through the string once.
- **Space Complexity:** O(n), since we store each character once in one of the rows.

---

## Optimized Using Cycle Calculation

### Intuition:
Instead of simulating each row and direction change, we can leverage the repeating pattern in the zigzag format. Characters can be grouped by cycle sections for more efficient construction.

### Steps:
1. Determine the cycle length, which is the period after which the pattern repeats (`2 * numRows - 2`).
2. For each row, pick characters located at indices that contribute to this row within each cycle.
3. Collect and concatenate these characters directly for the result.

### Python Code:
```python
def convert(s: str, numRows: int) -> str:
    # Edge case: If only one row, return the string itself.
    if numRows == 1 or numRows >= len(s):
        return s

    # String to hold the final result
    result = []
    cycle_len = 2 * numRows - 2

    # Handle each row
    for i in range(numRows):
        # Traverse through the given string to get characters belonging to this row
        for j in range(i, len(s), cycle_len):
            # Add the primary character in each cycle
            result.append(s[j])
            
            # If it's not the first or last row, add the diagonal character
            if i != 0 and i != numRows - 1 and j + cycle_len - 2 * i < len(s):
                result.append(s[j + cycle_len - 2 * i])

    # Join all parts to form the final result string
    return ''.join(result)
```

### Complexity Analysis:
- **Time Complexity:** O(n), because we process each character at most once.
- **Space Complexity:** O(n), for storing the result. 

Both methods effectively solve the problem, but the optimized approach directly constructs the final result by leveraging the zigzag cycle, which could be more intuitive for larger inputs.

