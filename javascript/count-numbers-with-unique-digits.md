# [Leetcode 357: Count Numbers with Unique Digits](https://leetcode.com/problems/count-numbers-with-unique-digits/)

## Solutions
1. [Recursive Approach](#recursive-approach)
2. [Dynamic Programming Approach](#dynamic-programming-approach)
3. [Mathematical Approach](#mathematical-approach)

## Recursive Approach

### Intuition
The problem is essentially about counting numbers with unique digits given a certain digit limit `n`. Starting with `0`, there are `10^n` possible numbers, so we'll need a way to check each one using a recursive process. 

For each position of a number, you either pick a digit that hasn't been used yet or stop, which directly leads to a combinatorics-like solution using recursive backtracking for digit selection.

### Solution

```javascript
function countNumbersWithUniqueDigits(n) {
    function backtrack(position, currentNumber, usedDigits) {
        if (position === n) {
            return 1;
        }

        let count = 1; // Count the number itself (implicitly choosing to stop)
        for (let digit = 0; digit < 10; digit++) {
            if (digit === 0 && position === 0) {
                continue; // Skip leading zero in multi-digit numbers
            }
            if (!usedDigits[digit]) {
                usedDigits[digit] = true; // Mark digit as used
                count += backtrack(position + 1, currentNumber * 10 + digit, usedDigits);
                usedDigits[digit] = false; // Unmark the digit for next iterations
            }
        }
        return count;
    }

    return backtrack(0, 0, new Array(10).fill(false));
}

console.log(countNumbersWithUniqueDigits(2)); // Output: 91
```

### Time Complexity
- **O(10^n)**: At each digit position, 10 choices. The computation happens recursively, analogous to filling `n` slots with unique digits.
  
### Space Complexity
- **O(n)**: Recursion stack could grow up to depth `n`.

## Dynamic Programming Approach

### Intuition
By using dynamic programming (DP), we recognize the subproblem of finding unique numbers of a smaller count of digits can help with larger numbers. Given `f(n)` as the count of valid numbers for a particular `n`, resolve this iteratively by saving previous results.

### Solution

```javascript
function countNumbersWithUniqueDigits(n) {
    if (n === 0) return 1;
    
    let dp = Array(n + 1).fill(0);
    dp[0] = 1;

    for (let i = 1; i <= n; i++) {
        let availableNumbers = 9; // The leading digit options: 1-9
        dp[i] = 9;
        for (let j = 0; j < i - 1; j++) {
            dp[i] *= availableNumbers;
            availableNumbers--;
        }
        dp[i] += dp[i - 1];
    }

    return dp[n];
}

console.log(countNumbersWithUniqueDigits(2)); // Output: 91
```

### Time Complexity
- **O(n)**: Filling up the `dp` array linearly based on `n`.

### Space Complexity
- **O(n)**: Extra array `dp` to store intermediate results.

## Mathematical Approach

### Intuition
Using a mathematical approach helps understand the combinatorial nature of the problem. For `n` problems, compute directly by counting permutations of choosing unique digits, respecting order constraints. It involves calculating the possible combinations of the digits at every position.

### Solution

```javascript
function countNumbersWithUniqueDigits(n) {
    if (n === 0) return 1;
    let count = 10, uniqueDigits = 9, availableDigits = 9;
    
    while (n > 1 && availableDigits > 0) {
        uniqueDigits *= availableDigits;
        count += uniqueDigits;
        availableDigits--;
        n--;
    }
    
    return count;
}

console.log(countNumbersWithUniqueDigits(2)); // Output: 91
```

### Time Complexity
- **O(1)**: A fixed number of operations, regardless of input `n`.

### Space Complexity
- **O(1)**: Constant space usage since no data structures depend on input size.

