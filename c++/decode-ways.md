# [Leetcode 91: Decode Ways](https://leetcode.com/problems/decode-ways/)

## Table of Contents
1. [Approach 1: Recursive Solution](#approach-1)
2. [Approach 2: Memoization (Top-Down DP)](#approach-2)
3. [Approach 3: Dynamic Programming (Bottom-Up)](#approach-3)
4. [Approach 4: Dynamic Programming with Constant Space](#approach-4)

---

## Approach 1: Recursive Solution

### Intuition

The problem is about decoding a string of digits into letters, where '1' translates to 'A', '2' translates to 'B', ..., and '26' translates to 'Z'. The simplest approach is to evaluate each character and see if it can be a standalone character or a pair of characters that provide a valid encoding.

We recursively check:
- If the current character can be decoded as a single digit.
- If two consecutive characters can form a valid double-digit number between 10 and 26.

This approach has a lot of redundant calculations, especially for long strings, which is why it's not efficient but serves as a good way to understand the problem.

```cpp
int numDecodingsRecursive(const std::string& s, int index) {
    // If we've reached the end of the string, there's a valid decoding
    if (index == s.size()) return 1;
    
    // A starting '0' is invalid
    if (s[index] == '0') return 0;
    
    // Consider the single digit
    int count = numDecodingsRecursive(s, index + 1);
    
    // Consider the double digits
    if (index < s.size() - 1) {
        int twoDigit = std::stoi(s.substr(index, 2));
        if (twoDigit >= 10 && twoDigit <= 26) {
            count += numDecodingsRecursive(s, index + 2);
        }
    }

    return count;
}

int numDecodings(std::string s) {
    return numDecodingsRecursive(s, 0);
}
```

### Complexity
- **Time Complexity:** O(2^n) where n is the length of the string. Each character potentially leads to two recursive calls.
- **Space Complexity:** O(n), depth of recursion stack.

---

## Approach 2: Memoization (Top-Down DP)

### Intuition

To improve upon the recursive solution, we can store results for subproblems and reuse them, rather than recalculating. This memoization avoids redundant calculations, making it more efficient.

```cpp
int numDecodingsMemo(const std::string& s, int index, std::vector<int>& memo) {
    if (index == s.size()) return 1;
    if (s[index] == '0') return 0;
    
    if (memo[index] != -1) return memo[index];
    
    int count = numDecodingsMemo(s, index + 1, memo);
    
    if (index < s.size() - 1) {
        int twoDigit = std::stoi(s.substr(index, 2));
        if (twoDigit >= 10 && twoDigit <= 26) {
            count += numDecodingsMemo(s, index + 2, memo);
        }
    }
    
    return memo[index] = count;
}

int numDecodings(std::string s) {
    std::vector<int> memo(s.size(), -1);
    return numDecodingsMemo(s, 0, memo);
}
```

### Complexity
- **Time Complexity:** O(n), where n is the length of the string.
- **Space Complexity:** O(n), used for the memoization array and recursion stack.

---

## Approach 3: Dynamic Programming (Bottom-Up)

### Intuition

Instead of relying on recursion and memoization, a bottom-up approach using Dynamic Programming allows us to iteratively calculate the number of ways to decode the message. We use a DP array where each entry represents the number of ways to decode up to that position.

```cpp
int numDecodings(std::string s) {
    int n = s.size();
    if (n == 0) return 0;
    
    std::vector<int> dp(n + 1, 0);
    dp[n] = 1;  // Base case; empty string has one way to be decoded
    
    for (int i = n - 1; i >= 0; i--) {
        if (s[i] == '0') continue; // A '0' at the start is invalid
        
        dp[i] = dp[i + 1];
        
        if (i < n - 1) {
            int twoDigit = std::stoi(s.substr(i, 2));
            if (twoDigit >= 10 && twoDigit <= 26) {
                dp[i] += dp[i + 2];
            }
        }
    }
    
    return dp[0];
}
```

### Complexity
- **Time Complexity:** O(n), where n is the length of the string.
- **Space Complexity:** O(n), used for the DP array.

---

## Approach 4: Dynamic Programming with Constant Space

### Intuition

Instead of maintaining a full DP array, we realize that we only need the last two computations at any time, reducing space complexity to O(1).

```cpp
int numDecodings(std::string s) {
    int n = s.size();
    if (n == 0 || s[0] == '0') return 0;
    
    int a = 1, b = 0, c = 0;
    
    for (int i = n - 1; i >= 0; i--) {
        if (s[i] == '0') {
            c = 0;
        } else {
            c = a;
            if (i < n - 1) {
                int twoDigit = std::stoi(s.substr(i, 2));
                if (twoDigit >= 10 && twoDigit <= 26) {
                    c += b;
                }
            }
        }
        b = a;
        a = c;
    }
    
    return a;
}
```

### Complexity
- **Time Complexity:** O(n), where n is the length of the string.
- **Space Complexity:** O(1), as only a few variables are used.

