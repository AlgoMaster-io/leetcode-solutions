# 902. [Numbers At Most N Given Digit Set](https://leetcode.com/problems/numbers-at-most-n-given-digit-set/)

## Approach 1: Simple Counting with Recursive Backtracking

### Solution
```cpp
// Time Complexity: O(n * m)
// Space Complexity: O(n)
// where n is the number of digits in N and m is the number of digits in D
class Solution {
public:
    int atMostNGivenDigitSet(vector<string>& digits, int n) {
        string nStr = to_string(n);
        int len = nStr.size();
        
        // Convert digit strings to a vector of characters
        vector<char> digitArr;
        for (const string& digit : digits) {
            digitArr.push_back(digit[0]);
        }
        
        return backtrack(digitArr, nStr, 0, true);
    }
    
private:
    int backtrack(const vector<char>& digits, const string& nStr, int index, bool limit) {
        if (index == nStr.size()) { // Base case: reached the length of nStr
            return 1;
        }
        
        int count = 0;
        for (char d : digits) {
            if (limit && d > nStr[index]) {
                break; // No need to consider digits greater than current nStr digit
            }
            count += backtrack(digits, nStr, index + 1, limit && d == nStr[index]);
        }
        
        // Add numbers with length less than nStr (full set combinations)
        if (!limit) {
            int factor = 1;
            for (int i = index + 1; i < nStr.size(); i++) {
                factor *= digits.size(); // for each position, we have `|digits|` choices 
            }
            count += factor;
        }
        
        return count;
    }
};
```

## Approach 2: Dynamic Programming

### Solution
```cpp
// Time Complexity: O(n * m)
// Space Complexity: O(n)
// where n is the number of digits in N and m is the number of digits in D
class Solution {
public:
    int atMostNGivenDigitSet(vector<string>& digits, int n) {
        string nStr = to_string(n);
        int len = nStr.size();
        vector<int> dp(len + 1, 0);
        dp[len] = 1;

        // Convert digit strings to a vector of characters
        vector<char> digitArr;
        for (const string& digit : digits) {
            digitArr.push_back(digit[0]);
        }

        for (int i = len - 1; i >= 0; i--) {
            char c = nStr[i];
            for (char d : digitArr) {
                if (d < c) {
                    dp[i] += pow(digits.size(), len - i - 1);
                } else if (d == c) {
                    dp[i] += dp[i + 1];
                }
            }
        }

        int result = 0;
        for (int i = 1; i < len; i++) {
            result += pow(digits.size(), i);
        }
        return result + dp[0];
    }
};
```

## Approach 3: Mathematical Counting

### Solution
```cpp
// Time Complexity: O(n * m)
// Space Complexity: O(1)
// where n is the number of digits in N and m is the number of digits in D
#include <vector>
#include <string>
#include <cmath>

class Solution {
public:
    int atMostNGivenDigitSet(vector<string>& digits, int n) {
        string nStr = to_string(n);
        int len = nStr.size();
        vector<vector<int>> count(len, vector<int>(2, 0));

        // Precompute the power array to avoid repetitive calculation
        vector<int> power(len + 1, 1);
        for (int i = 1; i <= len; i++) {
            power[i] = power[i - 1] * digits.size();
        }

        for (int i = 0; i < len; i++) {
            char c = nStr[i];
            for (const string& digit : digits) {
                char d = digit[0];

                if (d < c) {
                    count[i][0] += power[len - i - 1];
                } else if (d == c) {
                    count[i][1] = 1;
                }
            }
            if (count[i][1] == 0) break;
        }

        int result = 0;
        for (int i = 0; i < len; i++) {
            result += count[i][0];
        }

        for (int i = 1; i < len; i++) {
            result += power[i];
        }

        result += count[len - 1][1];

        return result;
    }
};
```

