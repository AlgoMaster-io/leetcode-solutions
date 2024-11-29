# 132. [Palindrome Partitioning II](https://leetcode.com/problems/palindrome-partitioning-ii/)

## Approach 1: Dynamic Programming with Palindrome Check

### Solution
```javascript
// Time Complexity: O(n^2)
// Space Complexity: O(n^2)
class Solution {
    minCut(s) {
        const n = s.length;
        const isPalindrome = Array.from({length: n}, () => Array(n).fill(false));
        
        // Fill the isPalindrome table
        for (let length = 1; length <= n; length++) {
            for (let i = 0; i <= n - length; i++) {
                const j = i + length - 1;
                if (s[i] === s[j]) {
                    isPalindrome[i][j] = length <= 2 || isPalindrome[i + 1][j - 1];
                }
            }
        }
        
        const cuts = new Array(n);
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
}
```

## Approach 2: Optimized with Less Space for Palindrome Check

### Solution
```javascript
// Time Complexity: O(n^2)
// Space Complexity: O(n)
class Solution {
    minCut(s) {
        const n = s.length;
        const cuts = new Array(n);
        const isPalindrome = new Array(n);
        
        for (let i = 0; i < n; i++) {
            let minCuts = i;
            for (let j = 0; j <= i; j++) {
                if (s[j] === s[i] && (i - j <= 1 || isPalindrome[j + 1])) {
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
}
```

