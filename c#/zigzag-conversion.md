# [Leetcode 6: Zigzag Conversion](https://leetcode.com/problems/zigzag-conversion/)

## Approaches:
1. [Naive Row-by-Row Simulation](#naive-row-by-row-simulation)
2. [Using StringBuilder for Efficiency](#using-stringbuilder-for-efficiency)
3. [Mathematical Index Calculation](#mathematical-index-calculation)

### Naive Row-by-Row Simulation

**Intuition**: 
The problem requires us to simulate the zigzag pattern formation of a given string across rows then concatenate them into a single string. The direct intuition is to simulate this formation by placing characters in their respective rows in a zigzag manner, then read across each row to form the resultant string.

**Approach**:
- Start with an empty list of rows.
- Use a direction flag to simulate zigzag (down-up direction) across rows.
- Iterate through the string, adding each character to the current row and changing direction when the top or bottom is reached.
  
```csharp
public class Solution {
    public string Convert(string s, int numRows) {
        if (numRows == 1 || s.Length <= numRows) return s;

        // Create each row as an empty StringBuilder
        List<StringBuilder> rows = new List<StringBuilder>();
        for (int i = 0; i < Math.Min(numRows, s.Length); i++)
            rows.Add(new StringBuilder());

        int currentRow = 0;
        bool goingDown = false;

        foreach (char c in s) {
            // Add character to current row
            rows[currentRow].Append(c);
            // Change direction when hitting the first or last row
            if (currentRow == 0 || currentRow == numRows - 1)
                goingDown = !goingDown;
            // Move up or down to the next row
            currentRow += goingDown ? 1 : -1;
        }

        // Combine all rows to produce final result string
        StringBuilder result = new StringBuilder();
        foreach (StringBuilder row in rows)
            result.Append(row);

        return result.ToString();
    }
}
```

**Time Complexity**: O(n), where n is the length of the string.
**Space Complexity**: O(n), for storing the zigzag pattern.

### Using StringBuilder for Efficiency

**Intuition**:
Continuing from the naive approach, each row can be represented more efficiently using the `StringBuilder` class in C#. This approach optimizes string concatenation operations.

**Approach**:
- Utilize `StringBuilder` for each row to efficiently build the zigzag.
- The logic for row simulation remains the same as the naive approach.

**Code and Complexity**: The code remains identical to the naive approach.

**Time Complexity**: O(n)
**Space Complexity**: O(n)

### Mathematical Index Calculation

**Intuition**:
Instead of simulating the zigzag insertion, mathematically calculate the position of each character based on its index and row number. This approach directly indexes into the string for more efficient traversal.

**Approach**:
- Recognize the cyclic nature of row filling.
- Calculate the positions in each row using a mathematical formula with step size based on pattern recurrence.

```csharp
public class Solution {
    public string Convert(string s, int numRows) {
        if (numRows == 1 || s.Length <= numRows) return s;

        StringBuilder result = new StringBuilder();

        int cycleLen = 2 * numRows - 2; // Cycle length for zigzag

        for (int i = 0; i < numRows; i++) {
            for (int j = 0; j + i < s.Length; j += cycleLen) {
                result.Append(s[j + i]); // Element in the column
                if (i != 0 && i != numRows - 1 && j + cycleLen - i < s.Length)
                    result.Append(s[j + cycleLen - i]); // Element in the diagonal
            }
        }

        return result.ToString();
    }
}
```

**Time Complexity**: O(n)
**Space Complexity**: O(n)

This approach efficiently calculates and combines elements without simulating row-by-row, making it well-suited for larger inputs.

