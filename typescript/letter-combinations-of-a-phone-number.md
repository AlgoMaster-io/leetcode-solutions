# [Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

## Approaches:
- [Basic Backtracking Approach](#basic-backtracking-approach)
- [Optimized Backtracking with Early Stopping Condition](#optimized-backtracking-with-early-stopping-condition)

### Basic Backtracking Approach

**Intuition:**

The classic problem of generating all letter combinations from a series of button presses on a phone keypad can be effectively solved with a backtracking algorithm. The fundamental idea is to explore all possible combinations by substituting each digit with its corresponding letters, recursing further until all digits are processed.

**Approach:**

1. Create a mapping of digits to their corresponding letters as found on a phone keypad.
2. Initialize a recursive function that will build combinations by iterating over each digit's corresponding letters.
3. Append each letter to a temporary path and recurse until the path length equals the input digits length.
4. When a valid path is found, add it to the result list.
5. This algorithm explores every possible combination, ensuring all potential combinations are constructed.

**Code:**

```typescript
function letterCombinations(digits: string): string[] {
    if (digits.length === 0) return [];

    const phoneMap: {[key: string]: string} = {
        '2': 'abc', '3': 'def', '4': 'ghi', '5': 'jkl',
        '6': 'mno', '7': 'pqrs', '8': 'tuv', '9': 'wxyz'
    };

    const res: string[] = [];
    // Helper function to perform backtracking
    function backtrack(index: number, path: string) {
        // Base condition: if the current path length equals digits length
        if (path.length === digits.length) {
            res.push(path);
            return;
        }
        // Fetch chars for current digit
        const letters = phoneMap[digits[index]];
        // Explore each letter
        for (const letter of letters) {
            backtrack(index + 1, path + letter);
        }
    }
    backtrack(0, '');
    return res;
}
```

**Complexity:**
- Time Complexity: \( O(3^N \times 4^M) \), where \( N \) is the number of digits that have 3 corresponding letters ('2', '3', '4', '5', '6', '8') and \( M \) is the number of digits with 4 letters ('7', '9').
- Space Complexity: \( O(3^N \times 4^M) \), as we store all unique combinations.

### Optimized Backtracking with Early Stopping Condition

**Intuition:**

The above approach can’t really be optimized in terms of time complexity since it involves generating all possible combinations. However, we can make subtle improvements for early rejection in specific cases, although these will have marginal improvement in practical applications but are worth mentioning for their potential to improve the backtracking algorithm's efficiency.

**Approach:**

- Use memoization to store already computed results for specific subproblems in case of repeated calls. Here, memoization is not necessarily helpful without overlapping subproblems, but imagine processing parts of other numbers repeatedly.
  
- Although not implemented directly in this scenario, memoization would indeed become relevant if this solution was part of a larger solution set requiring multiple independent, reflected lookups or cross-number pattern optimizations.

**Code:**

The memoization isn't particularly required or beneficial due to the nature of pure combinatorial generation in this specific problem, but the framework below shows how such a system might be started:

```typescript
function letterCombinations(digits: string): string[] {
    if (digits.length === 0) {
        return [];
    }

    const phoneMap: {[key: string]: string} = {
        '2': 'abc', '3': 'def', '4': 'ghi', '5': 'jkl',
        '6': 'mno', '7': 'pqrs', '8': 'tuv', '9': 'wxyz'
    };

    const memo: {[key: string]: string[]} = {};

    function means(d: string): string[] {
        // Check memo
        if (memo[d]) return memo[d];

        // Compute combinations via backtracking
        const res: string[] = [];
        const chars = phoneMap[d];
        for (let char of chars) {
            res.push(char);
            memo[d] = res;
        }
        return res;
    }
    const res: string[] = [];
    // Helper function to perform backtracking
    function backtrack(index: number, path: string) {
        if (path.length === digits.length) {
            res.push(path);
            return;
        }
        const letters = means(digits[index]);
        for (const letter of letters) {
            backtrack(index + 1, path + letter);
        }
    }
    backtrack(0, '');
    return res;
}
```

**Complexity:**
- Time and Space Complexity remain the same as traditional backtracking due to the need to explore all combinations.

Memoization becomes useful and has more impact in environments with overlapping subproblems, which does not apply heavily to this case. Nonetheless, understanding how to incorporate such methods remains valuable.

