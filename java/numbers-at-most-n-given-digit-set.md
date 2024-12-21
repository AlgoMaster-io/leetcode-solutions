# [LeetCode 902: Numbers At Most N Given Digit Set](https://leetcode.com/problems/numbers-at-most-n-given-digit-set/)

## Approaches
1. [Brute Force Enumeration](#approach-1-brute-force-enumeration)
2. [Dynamic Programming with Digit DP](#approach-2-dynamic-programming-with-digit-dp)
3. [Mathematical Counting](#approach-3-mathematical-counting)

---

## Approach 1: Brute Force Enumeration

### Intuition:
The brute force method involves generating all possible numbers from the given digit set and checking if they are less than or equal to the target number `N`. This approach is based on generating permutations of increasing lengths and counting how many are valid.

### Steps:
1. Convert the integer `N` to a string to easily access individual digits.
2. Generate all numbers of lengths from 1 up to the length of `N` using the digits.
3. For each generated number, check if it is less than or equal to `N` and count it.

```java
public int atMostNGivenDigitSet(String[] D, int N) {
    // Convert N to a string for easier digit comparisons
    String nStr = Integer.toString(N);
    int nLen = nStr.length();
    
    int count = 0;
    // Generate numbers with length less than nLen
    for (int len = 1; len < nLen; len++) {
        count += Math.pow(D.length, len);
    }
    
    // Generate numbers of length nLen
    count += helper(D, nStr.toCharArray(), 0, true);
    
    return count;
}

private int helper(String[] D, char[] nStr, int pos, boolean isTight) {
    if (pos == nStr.length) {
        return 1;
    }

    int res = 0;
    for (String digitStr : D) {
        char digit = digitStr.charAt(0);
        if (digit < nStr[pos] || (digit == nStr[pos] && isTight)) {
            res += helper(D, nStr, pos + 1, isTight && digit == nStr[pos]);
        }
        if (digit > nStr[pos] && isTight) {
            break;
        }
    }
    
    return res;
}
```

### Complexity:
- **Time Complexity:** `O((D.length)^(log10 N))` where `log10 N` is the number of digits in `N`.
- **Space Complexity:** `O(log10 N)` for the recursive stack.

---

## Approach 2: Dynamic Programming with Digit DP

### Intuition:
This approach leverages digit dynamic programming to count the valid numbers directly instead of generating and validating them one by one. We pre-compute the numbers using dynamic programming, breaking the problem into manageable sub-problems.

### Steps:
1. Define a recursive function with memoization to solve for smaller sub-problems.
2. Use the DP table to store results of previously computed states.
3. Accumulate results from the DP table to get the final count of valid numbers.

```java
public int atMostNGivenDigitSet(String[] D, int N) {
    char[] nStr = Integer.toString(N).toCharArray();
    int nLen = nStr.length;
    int[][] dp = new int[nLen + 1][2];
    // Fill dp with -1 to denote uncomputed states
    for (int[] row : dp) {
        Arrays.fill(row, -1);
    }
    return dpHelper(D, nStr, 0, true, dp) - 1;
}

private int dpHelper(String[] D, char[] nStr, int pos, boolean isTight, int[][] dp) {
    if (pos == nStr.length) {
        return 1;
    }
    
    if (dp[pos][isTight ? 1 : 0] != -1) {
        return dp[pos][isTight ? 1 : 0];
    }
    
    int limit = isTight ? nStr[pos] - '0' : 9;
    int res = 0;
    
    for (String dStr : D) {
        int d = dStr.charAt(0) - '0';
        if (d <= limit) {
            res += dpHelper(D, nStr, pos + 1, isTight && d == limit, dp);
        }
    }
    
    return dp[pos][isTight ? 1 : 0] = res;
}
```

### Complexity:
- **Time Complexity:** `O(nLen * D.length)` where `nLen` is the number of digits in `N`.
- **Space Complexity:** `O(nLen * D.length)` for memoization table.

---

## Approach 3: Mathematical Counting

### Intuition:
Instead of generating numbers one-by-one, this approach leverages counting principles. We break down the problem by counting numbers having fewer digits than `N` and exactly digits of `N`.

### Steps:
1. Count numbers of all lengths smaller than the length of `N`.
2. Use prefix-based digit-by-digit comparison to generate valid numbers of the same length as `N`.
3. Accumulate the count based on the above computations.

```java
public int atMostNGivenDigitSet(String[] D, int N) {
    String nStr = Integer.toString(N);
    int nLen = nStr.length();
    int dLen = D.length;
    
    // Count numbers of length less than nLen
    int count = 0;
    for (int i = 1; i < nLen; i++) {
        count += Math.pow(dLen, i);
    }
    
    // Count numbers with the same length as N
    for (int i = 0; i < nLen; i++) {
        boolean isSameDigitFound = false;
        for (String dStr : D) {
            if (dStr.charAt(0) < nStr.charAt(i)) {
                count += Math.pow(dLen, nLen - i - 1);
            } else if (dStr.charAt(0) == nStr.charAt(i)) {
                isSameDigitFound = true;
                break;
            }
        }
        if (!isSameDigitFound) return count;
    }
    
    return count + 1; // Include the number N itself
}
```

### Complexity:
- **Time Complexity:** `O(nLen * D.length)` where `nLen` is the number of digits in `N`.
- **Space Complexity:** `O(1)` no extra space required beyond variables.

