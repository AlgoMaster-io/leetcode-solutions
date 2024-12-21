[Leetcode Problem: Zigzag Conversion](https://leetcode.com/problems/zigzag-conversion/)

### Approaches:
- [Approach 1: Zigzag Pattern Simulation](#approach-1-zigzag-pattern-simulation)

---

#### Approach 1: Zigzag Pattern Simulation

**Intuition:**

The problem asks to reorder the given string in a specified zigzag pattern across multiple rows. The basic idea here is direct simulation by creating an array of strings, each representing a row in the zigzag pattern.

- We iterate over each character in the string and decide which row of the zigzag it belongs to.
- This is determined by moving up and down the rows in a sequential pattern.
- Once the pattern is built, we can concatenate all the rows to get the final string.

**Implementation:**

```java
class Solution {
    public String convert(String s, int numRows) {
        // Edge case: if the zigzag has only one row, the result is the string itself.
        if (numRows == 1) return s;
        
        // Create an array of StringBuilder for each row.
        StringBuilder[] rows = new StringBuilder[numRows];
        for (int i = 0; i < numRows; i++) {
            rows[i] = new StringBuilder();
        }
        
        // Current position and direction of traversal.
        int currentRow = 0;
        boolean goingDown = false;
        
        // Traverse the string and fill each row in the zigzag pattern.
        for (char c : s.toCharArray()) {
            // Append current character to the current row.
            rows[currentRow].append(c);
            
            // If we're at the first or the last row, change direction.
            if (currentRow == 0 || currentRow == numRows - 1) goingDown = !goingDown;
            
            // Move to the next row in the current direction.
            currentRow += goingDown ? 1 : -1;
        }
        
        // Combine all rows into one string.
        StringBuilder result = new StringBuilder();
        for (StringBuilder row : rows) {
            result.append(row);
        }
        
        return result.toString();
    }
}
```

**Time Complexity:** \(O(n)\) where \(n\) is the length of the input string. Each character is processed once.

**Space Complexity:** \(O(n)\) to store the strings in the StringBuilder objects.

The above solution effectively simulates the zigzag formation in the intended pattern by using multiple `StringBuilder` objects to represent each row of the zigzag. By keeping track of the current row and direction, we can build the appropriate zigzag shape. After processing all characters, concatenating all `StringBuilder` provides the final result.

