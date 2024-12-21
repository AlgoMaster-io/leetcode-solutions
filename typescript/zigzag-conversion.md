# [Leetcode 6: Zigzag Conversion](https://leetcode.com/problems/zigzag-conversion/)

## Solutions
- [Brute Force Approach](#brute-force-approach)
- [Array of Strings Approach](#array-of-strings-approach)
- [Optimized String Builder Approach](#optimized-string-builder-approach)

### Brute Force Approach

#### Intuition:
The simplest way to solve this problem is to simulate the zigzag pattern row by row. We can construct each row by iterating over the original string in a way that factors in the zigzag pattern. This approach, however, isn't the most efficient because of the need to repeatedly traverse parts of the string.

#### Code:
```typescript
function convertBruteForce(s: string, numRows: number): string {
    if (numRows === 1 || s.length <= numRows) {
        return s;
    }

    const step = 2 * numRows - 2;
    let result = '';
    
    // Construct each row separately
    for (let row = 0; row < numRows; row++) {
        for (let i = row; i < s.length; i += step) {
            result += s[i];

            // Calculate the index of the "zigzag" character
            let zigzagIndex = i + step - 2 * row;
            if (row !== 0 && row !== numRows - 1 && zigzagIndex < s.length) {
                result += s[zigzagIndex];
            }
        }
    }
    return result;
}
```
#### Time Complexity:
- **O(n)**, where \( n \) is the length of the string. Every character is processed once.
  
#### Space Complexity:
- **O(n)**, where \( n \) is the length of the string for the resulting string.

### Array of Strings Approach

#### Intuition:
Instead of constructing the result for each row individually during each iteration, we can maintain an array of strings where each index corresponds to a row. We then iterate through the input string to simulate the zigzag movement and fill each row string at once.

#### Code:
```typescript
function convertArrayOfStrings(s: string, numRows: number): string {
    if (numRows === 1 || s.length <= numRows) {
        return s;
    }

    const rows: string[] = Array.from({ length: numRows }, () => '');
    let currentRow = 0;
    let direction = -1;
    
    for (let c of s) {
        rows[currentRow] += c;

        // Change direction if reaching the top or bottom
        if (currentRow === 0 || currentRow === numRows - 1) {
            direction *= -1;
        }

        currentRow += direction;
    }

    // Concatenate all rows to get the final result
    return rows.join('');
}
```
#### Time Complexity:
- **O(n)**, where \( n \) is the length of the string. We iterate over the string once.
  
#### Space Complexity:
- **O(n)**; results are stored in an array of strings.

### Optimized String Builder Approach

#### Intuition:
For optimal performance, we use a similar approach to the "Array of Strings", but to avoid the overhead of string concatenation within the loop, we can use a list or array for constructing the result and only join them at the very end. This minimizes the computational overhead.

#### Code:
```typescript
function convertOptimizedStringBuilder(s: string, numRows: number): string {
    if (numRows === 1 || s.length <= numRows) {
        return s;
    }

    const rows = Array.from({ length: Math.min(numRows, s.length) }, () => []);
    let currentRow = 0;
    let goingDown = false;

    for (let c of s) {
        rows[currentRow].push(c);

        if (currentRow === 0 || currentRow === numRows - 1) {
            goingDown = !goingDown;
        }

        currentRow += goingDown ? 1 : -1;
    }

    // Construct the final result
    let result = '';
    for (let row of rows) {
        result += row.join('');
    }
    return result;
}
```
#### Time Complexity:
- **O(n)**, where \( n \) is the length of the string. 

#### Space Complexity:
- **O(n)**; results are constructed in an array of arrays. 

This concludes the solutions for the Zigzag Conversion problem with different approaches and optimizations described. Choose the one most suitable for your needs based on the constraints.

