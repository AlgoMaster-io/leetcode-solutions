# [Palindrome Partitioning II](https://leetcode.com/problems/palindrome-partitioning-ii/)

## Approaches
- [Approach 1: Brute Force with Dynamic Programming](#approach-1-brute-force-with-dynamic-programming)
- [Approach 2: Optimized Dynamic Programming with Preprocessing Palindrome](#approach-2-optimized-dynamic-programming-with-preprocessing-palindrome)

---

## Approach 1: Brute Force with Dynamic Programming

### Intuition

The problem asks for the minimum number of cuts needed for a palindrome partitioning of a string `s`. A palindrome is a string that reads the same forward and backward. Our task is to partition the string into as few palindromic components as possible.

A straightforward approach would involve checking every possible substring to see if it's a palindrome, which can be very inefficient. Instead, we can use dynamic programming to store results of subproblems, hence optimizing the checks for palindromes.

### Algorithm

1. **Initialize the DP Table**: We create a DP table `dp` where `dp[i]` represents the minimum number of cuts needed for the substring `s[0...i]`.

2. **Base Case**: `dp[0] = 0` since a single character is always a palindrome and requires zero cuts.

3. **Iterate through the string**: For each character position `i`, we attempt to partition the string at every position `j` before `i` and check if the substring `s[j...i]` is a palindrome.

4. **Check Palindrome Substring**: Create an auxiliary function or method to determine if a substring `s[j...i]` is a palindrome.

5. **Update DP Table**: If `s[j...i]` is a palindrome, update `dp[i]` with the minimum cuts found so far.

```javascript
function minCut(s) {
    const n = s.length;
    const dp = new Array(n).fill(0);
    
    for (let i = 0; i < n; i++) {
        dp[i] = i; // maximum cuts is the length itself (all single char partitions)
        for (let j = 0; j <= i; j++) {
            // Check if s[j...i] is a palindrome
            if (isPalindrome(s, j, i)) {
                // If this is palindromic, compute the cuts
                dp[i] = j === 0 ? 0 : Math.min(dp[i], dp[j - 1] + 1);
            }
        }
    }
    
    return dp[n - 1];
}

function isPalindrome(s, l, r) {
    while (l < r) {
        if (s[l] !== s[r]) return false;
        l++;
        r--;
    }
    return true;
}
```

### Time Complexity

- **Time Complexity**: O(n^3) due to the nested loop and palindrome checks
- **Space Complexity**: O(n) for the DP array

---

## Approach 2: Optimized Dynamic Programming with Preprocessing Palindrome

### Intuition

We can improve our first approach by precomputing whether each possible substring is a palindrome. This preprocessing will let us check palindrome status in constant time.

### Algorithm

1. **Create a 2D Table**: `isPal[i][j]` to store whether the substring `s[i...j]` is a palindrome.

2. **Fill the 2D Table**: Use the following rules:
   - `isPal[i][i]` is true (single characters are palindromes).
   - `isPal[i][i+1]` is true if `s[i] === s[i+1]`.
   - For longer substrings, `isPal[i][j]` is true if `s[i] === s[j]` and `isPal[i+1][j-1]` is true.

3. **Dynamic Programming on Minimum Cuts**: Using the precomputed `isPal` table, calculate the `dp` array as in Approach 1, but now palindrome checks are O(1).

```javascript
function minCut(s) {
    const n = s.length;
    const dp = new Array(n).fill(0);
    const isPal = Array.from({ length: n }, () => new Array(n).fill(false));
    
    // Precompute palindrome table
    for (let i = 0; i < n; i++) {
        for (let j = 0; j <= i; j++) {
            if (s[i] === s[j] && (i - j < 2 || isPal[j + 1][i - 1])) {
                isPal[j][i] = true;
            }
        }
    }
    
    for (let i = 0; i < n; i++) {
        if (isPal[0][i]) {
            dp[i] = 0;
        } else {
            dp[i] = Infinity;
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

### Time Complexity

- **Time Complexity**: O(n^2) for both palindrome checks and building the dp array.
- **Space Complexity**: O(n^2) for the palindrome table.

This optimized approach significantly improves the checking of palindromic substrings, leading to faster computation times for larger input sizes.

