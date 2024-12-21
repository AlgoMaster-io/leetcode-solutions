### [Leetcode Problem 6: Zigzag Conversion](https://leetcode.com/problems/zigzag-conversion/)

- [Approach 1: Simulated Row-by-Row Construction](#approach-1-simulated-row-by-row-construction)
- [Approach 2: Optimized Direct Calculation](#approach-2-optimized-direct-calculation)

---

#### Approach 1: Simulated Row-by-Row Construction

**Intuition:**

The idea is to simulate the process of writing the characters in a zigzag format, just as you'd write on paper. We can use an array of strings to represent the rows. While iterating through the input string, we'll determine the appropriate row for each character and append it accordingly. We'll need to change direction when we reach the topmost or bottom-most row.

**Steps:**

1. Edge Check: If the number of rows is 1 or greater than the length of the string, return the original string since zigzag conversion is unnecessary.
2. Create an array of strings for each row.
3. Initialize variables to track the current row and direction (whether going down or up).
4. Traverse each character in the input string:
   - Append the character to the corresponding row.
   - Adjust the current row and check for direction change at the first or last row.
5. Concatenate all the strings in the row array to build the final output string.

**Code:**

```cpp
class Solution {
public:
    string convert(string s, int numRows) {
        // Case where zigzag effect is not applicable
        if (numRows == 1 || numRows >= s.size()) return s;
        
        // Create a vector of strings for each row
        vector<string> rows(numRows);
        // Initialize the current row and direction
        int currentRow = 0;
        bool goingDown = false;
        
        // Traverse each character of the input string
        for (char c : s) {
            rows[currentRow] += c; // Add character to the current row
            // Change direction if we're on the first or last row
            if (currentRow == 0 || currentRow == numRows - 1) goingDown = !goingDown;
            currentRow += goingDown ? 1 : -1;
        }
        
        // Concatenate all rows into a single string
        string result;
        for (string row : rows) {
            result += row;
        }
        return result;
    }
};
```

**Complexity Analysis:**

- **Time Complexity:** \(O(n)\), where \(n\) is the length of the string. We traverse each character once.
- **Space Complexity:** \(O(n)\). We use additional storage for the row strings.

---

#### Approach 2: Optimized Direct Calculation

**Intuition:**

Instead of simulating the full zigzag process, we can directly calculate how to build the zigzag pattern by understanding its periodicity. We can determine which characters appear in which result order position directly.

**Steps:**

1. The zigzag pattern repeats every cycle of \(2 \times \text{numRows} - 2\).
2. For each row, identify the characters that should appear in that row based on the calculated cycle.
3. Traverse through the string, directly filling in the result based on pattern repetition.

**Code:**

```cpp
class Solution {
public:
    string convert(string s, int numRows) {
        if (numRows == 1 || numRows >= s.size()) return s;
        
        string result;
        int cycleLen = 2 * numRows - 2;
        
        for (int i = 0; i < numRows; i++) {
            for (int j = 0; j + i < s.size(); j += cycleLen) {
                result += s[j + i]; // Main vertical section
                // Diagonal characters (except for the first and last row)
                if (i != 0 && i != numRows - 1 && j + cycleLen - i < s.size()) {
                    result += s[j + cycleLen - i];
                }
            }
        }
        
        return result;
    }
};
```

**Complexity Analysis:**

- **Time Complexity:** \(O(n)\), where \(n\) is the length of the string. We access each character potentially multiple times depending on row positioning.
- **Space Complexity:** \(O(n)\). The space used by the resulting zigzag string.

