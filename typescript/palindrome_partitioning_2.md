# 132. [Palindrome Partitioning II](https://leetcode.com/problems/palindrome-partitioning-ii/)

## Approach 1: Dynamic Programming with Palindrome Check

### Solution
typescript
```typescript
// Time Complexity: O(n^2)
// Space Complexity: O(n^2)
function minCut(s: string): number {
    const n = s.length;
    const isPalindrome: boolean[][] = Array.from({ length: n }, () => Array(n).fill(false));

    // Fill the isPalindrome table
    for (let length = 1; length <= n; length++) {
        for (let i = 0; i <= n - length; i++) {
            const j = i + length - 1;
            if (s.charAt(i) === s.charAt(j)) {
                isPalindrome[i][j] = length <= 2 || isPalindrome[i + 1][j - 1];
            }
        }
    }

    const cuts: number[] = Array(n).fill(0);
    for (let i = 0; i < n; i++) {
        if (isPalindrome[0][i]) {
            cuts[i] = 0;
        } else {
            cuts[i] = i;
            for (let j = 0; j < i; j++) {
                if (isPalindrome[j + 1][i]) {
                    cuts[i] = Math.min(cuts[i], cuts[j] + 1);
                }
            }
        }
    }

    return cuts[n - 1];
}
```

## Approach 2: Optimized with Less Space for Palindrome Check

### Solution
typescript
```typescript
// Time Complexity: O(n^2)
// Space Complexity: O(n)
function minCut(s: string): number {
    const n = s.length;
    const cuts: number[] = Array(n).fill(0);
    const isPalindrome: boolean[] = Array(n).fill(false);

    for (let i = 0; i < n; i++) {
        let minCuts = i;
        for (let j = 0; j <= i; j++) {
            if (s.charAt(j) === s.charAt(i) && (i - j <= 1 || isPalindrome[j + 1])) {
                isPalindrome[j] = true;
                minCuts = Math.min(minCuts, j === 0 ? 0 : cuts[j - 1] + 1);
            } else {
                isPalindrome[j] = false;
            }
        }
        cuts[i] = minCuts;
    }

    return cuts[n - 1];
}
```

