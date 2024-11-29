# 921. [Minimum Add to Make Parentheses Valid](https://leetcode.com/problems/minimum-add-to-make-parentheses-valid/)

## Approach: Counting Mismatches

### Solution
typescript
```typescript
// Time Complexity: O(n), where n is the length of the string
// Space Complexity: O(1)
function minAddToMakeValid(s: string): number {
    let openCount = 0; // Tracks unmatched '('
    let closeCount = 0; // Tracks unmatched ')'

    for (let i = 0; i < s.length; i++) {
        const c = s[i];
        if (c === '(') {
            openCount++; // Increment unmatched '('
        } else if (c === ')') {
            if (openCount > 0) {
                openCount--; // Match ')' with a previous '('
            } else {
                closeCount++; // Increment unmatched ')'
            }
        }
    }

    // Total unmatched parentheses is the sum of unmatched '(' and ')'
    return openCount + closeCount;
}
```

