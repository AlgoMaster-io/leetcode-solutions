# [Leetcode 132: Palindrome Partitioning II](https://leetcode.com/problems/palindrome-partitioning-ii/)

## Solutions
- [Solution 1: Recursive + Memoization Approach](#solution-1-recursive--memoization-approach)
- [Solution 2: Dynamic Programming Approach](#solution-2-dynamic-programming-approach)

---

## Solution 1: Recursive + Memoization Approach

### Intuition
The simplest way to solve this problem is to use recursion and check all possible ways to partition the string such that every substring is a palindrome. We use memoization to avoid recalculating results for the same subproblems, thereby optimizing the solution compared to pure recursion.

### Approach
1. Define a recursive function `minCuts` which returns the minimum cuts needed for a substring.
2. Use a helper function `isPalindrome(l, r)` to check if the substring `s[l...r]` is a palindrome.
3. Use a memoization array to store the results of subproblems to avoid redundant calculations.

### Time Complexity
- The time complexity is approximately O(n^2), where n is the length of the string because we are checking all possible substrings.

### Space Complexity
- The space complexity is O(n^2) due to the memoization table.

### Solution Code

```typescript
function minCut(s: string): number {
    const n = s.length;
    const memo = new Array(n).fill(-1).map(() => new Array(n).fill(-1));
    
    const isPalindrome = (l: number, r: number): boolean => {
        while (l < r) {
            if (s[l] !== s[r]) return false;
            l++;
            r--;
        }
        return true;
    };

    const minCuts = (start: number): number => {
        if (start === n) return 0;

        if (memo[start] !== -1) return memo[start];

        let minCut = Infinity;
        for (let end = start; end < n; end++) {
            if (isPalindrome(start, end)) {
                const cuts = 1 + minCuts(end + 1);
                minCut = Math.min(minCut, cuts);
            }
        }

        memo[start] = minCut;
        return memo[start];
    };

    // Since we always add 1 more for the partition we need to return -1
    return minCuts(0) - 1;
}
```

---

## Solution 2: Dynamic Programming Approach

### Intuition
Instead of recursive calls, we can iteratively fill a DP table where `dp[i]` represents the minimum cuts needed for substring `s[0...i]`. Use a helper table to precompute whether each substring is a palindrome.

### Approach
1. Precompute a 2D table `isPal` where `isPal[i][j]` represents if `s[i...j]` is a palindrome.
2. Use a DP array `dp` where `dp[i]` indicates the minimum cuts needed for `s[0...i]`.
3. Fill this table in a bottom-up manner using the precomputed `isPal` table.
4. The result will be in `dp[n-1]`.

### Time Complexity
- The time complexity here is O(n^2) due to filling up of both `dp` and palindrome tables.

### Space Complexity
- The space complexity is O(n^2) due to the `isPal` table.

### Solution Code

```typescript
function minCut(s: string): number {
    const n = s.length;
    const dp = new Array(n).fill(Infinity);
    const isPal = new Array(n).fill(false).map(() => new Array(n).fill(false));

    // Precompute palindrome information
    for (let end = 0; end < n; end++) {
        for (let start = 0; start <= end; start++) {
            if (s[start] === s[end] && (end - start <= 2 || isPal[start + 1][end - 1])) {
                isPal[start][end] = true;
            }
        }
    }

    for (let i = 0; i < n; i++) {
        if (isPal[0][i]) {
            dp[i] = 0; // If the whole substring is palindrome, no cut needed
        } else {
            for (let j = 0; j < i; j++) {
                if (isPal[j + 1][i]) {
                    dp[i] = Math.min(dp[i], dp[j] + 1);
                }
            }
        }
    }

    return dp[n - 1];
}
```


