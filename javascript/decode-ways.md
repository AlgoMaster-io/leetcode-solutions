# [Leetcode 91: Decode Ways](https://leetcode.com/problems/decode-ways/)

## Approaches
- [Recursive (Brute Force)](#recursive-brute-force)
- [Recursive with Memoization](#recursive-with-memoization)
- [Dynamic Programming (Bottom-Up)](#dynamic-programming-bottom-up)

---

### Recursive (Brute Force)

**Intuition**:  
The idea here is similar to the decision tree. From any given digit, we have the option to interpret it either as a single character or pair it with the next character if it's a valid number (i.e., between "10" and "26"). We recursively calculate the number of ways for each choice.

```javascript
function numDecodings(s) {
    function decodeWays(index) {
        // Base case: If we've reached the end of the string, there's one valid way to decode it.
        if (index === s.length) {
            return 1;
        }

        // If the current character is '0', it can't form a valid character
        if (s[index] === '0') {
            return 0;
        }

        // Option 1: Decode a single character
        let way1 = decodeWays(index + 1);

        // Option 2: Decode two characters
        let way2 = 0;
        if (index + 1 < s.length && (s[index] === '1' || (s[index] === '2' && s[index + 1] <= '6'))) {
            way2 = decodeWays(index + 2);
        }

        // Total ways from this index onwards
        return way1 + way2;
    }

    return decodeWays(0);
}
```

**Time Complexity**: \(O(2^n)\), where \(n\) is the length of the string, because there are two recursive paths for each index.  
**Space Complexity**: \(O(n)\), due to the recursion call stack.

---

### Recursive with Memoization

**Intuition**:  
To optimize the recursive approach, we use memoization to store previously computed results for any given index to avoid duplicate calculations.

```javascript
function numDecodings(s) {
    const memo = {};

    function decodeWays(index) {
        // Base case
        if (index === s.length) {
            return 1;
        }

        if (s[index] === '0') {
            return 0;
        }

        if (index in memo) {
            return memo[index];
        }

        // Option 1: Decode a single character
        let way1 = decodeWays(index + 1);

        // Option 2: Decode two characters
        let way2 = 0;
        if (index + 1 < s.length && (s[index] === '1' || (s[index] === '2' && s[index + 1] <= '6'))) {
            way2 = decodeWays(index + 2);
        }

        // Store the result in memo before returning
        memo[index] = way1 + way2;
        return memo[index];
    }

    return decodeWays(0);
}
```

**Time Complexity**: \(O(n)\), where \(n\) is the length of the string, since each index is computed only once due to memoization.  
**Space Complexity**: \(O(n)\), due to the memoization storage and recursion call stack.

---

### Dynamic Programming (Bottom-Up)

**Intuition**:  
Using dynamic programming, we build a table `dp` where `dp[i]` represents the number of ways to decode the substring from index `i` to the end. The value is calculated using the transitions similar to the recursive approach.

```javascript
function numDecodings(s) {
    // Edge case: empty string or starts with '0'
    if (s.length === 0 || s[0] === '0') {
        return 0;
    }

    let n = s.length;
    let dp = Array(n + 1).fill(0);

    // Base cases
    dp[n] = 1; // An empty string has one way to be decoded - not decoding anything.

    // Fill the dp array from the back
    for (let i = n - 1; i >= 0; i--) {
        if (s[i] !== '0') {
            // Single character decoding
            dp[i] = dp[i + 1];

            // Double character decoding
            if (i < n - 1 && (s[i] === '1' || (s[i] === '2' && s[i + 1] <= '6'))) {
                dp[i] += dp[i + 2];
            }
        }
    }

    return dp[0];
}
```

**Time Complexity**: \(O(n)\), where \(n\) is the length of the string, because we traverse the string once.  
**Space Complexity**: \(O(n)\), due to the dp array used to store results for subproblems.

---

