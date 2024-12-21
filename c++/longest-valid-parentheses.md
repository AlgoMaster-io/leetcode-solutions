## [Leetcode Problem 32: Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/)

### Approaches:
- [Naive Brute Force Approach](#naive-brute-force-approach)
- [Dynamic Programming Approach](#dynamic-programming-approach)
- [Using Stack](#using-stack)
- [Two-Pointer Approach](#two-pointer-approach)

---

### Naive Brute Force Approach

In the naive brute force approach, we consider all possible substrings of the given string and check if they are valid parentheses. The longest valid substring found gives us our result.

#### Code:
```cpp
class Solution {
public:
    bool isValid(const string& s) {
        // Use a counter to validate the parentheses
        int balance = 0;
        for (char c : s) {
            if (c == '(') {
                balance++;
            } else {
                balance--;
                // If balance goes negative, ')' are more than '('
                if (balance < 0) return false;
            }
        }
        // Valid if balance is zero
        return balance == 0;
    }
    
    int longestValidParentheses(string s) {
        int max_len = 0;

        // Check every possible substring
        for (int i = 0; i < s.length(); ++i) {
            for (int j = i + 2; j <= s.length(); j += 2) { // Consider even length substrings only
                if (isValid(s.substr(i, j - i))) {
                    max_len = max(max_len, j - i);
                }
            }
        }
        return max_len;
    }
};
```

#### Time Complexity:
- **O(n^3)** - Checking all substrings and validating each takes cubic time.
- **Space Complexity: O(n)** - Due to the call to `substr()` and new string creation.

---

### Dynamic Programming Approach

Using Dynamic Programming, we maintain an array `dp` where `dp[i]` stores the length of the longest valid parentheses substring ending at `i`.

#### Code:
```cpp
class Solution {
public:
    int longestValidParentheses(string s) {
        int n = s.length();
        if (n == 0) return 0;
        
        // DP array
        vector<int> dp(n, 0);
        int max_len = 0;
        
        for (int i = 1; i < n; ++i) {
            if (s[i] == ')') {
                if (s[i - 1] == '(') {
                    // Case "()", valid substring
                    dp[i] = (i >= 2 ? dp[i - 2] : 0) + 2;
                } else if (i - dp[i - 1] > 0 && s[i - dp[i - 1] - 1] == '(') {
                    // Case "()(()())"
                    dp[i] = dp[i - 1] + ((i - dp[i - 1]) >= 2 ? dp[i - dp[i - 1] - 2] : 0) + 2;
                }
                max_len = max(max_len, dp[i]);
            }
        }
        return max_len;
    }
};
```

#### Time Complexity:
- **O(n)** - We make a single pass through the string.
- **Space Complexity: O(n)** - For the DP array.

---

### Using Stack

We utilize a stack to keep track of indices of the parentheses. This helps in calculating valid lengths by popping elements when we match a valid pair.

#### Code:
```cpp
class Solution {
public:
    int longestValidParentheses(string s) {
        stack<int> st;
        st.push(-1); // Base for first valid substring calculation
        int max_len = 0;
        
        for (int i = 0; i < s.length(); ++i) {
            if (s[i] == '(') {
                st.push(i);
            } else {
                st.pop();
                if (st.empty()) {
                    st.push(i);
                } else {
                    max_len = max(max_len, i - st.top());
                }
            }
        }
        return max_len;
    }
};
```

#### Time Complexity:
- **O(n)** - Single pass with stack operations.
- **Space Complexity: O(n)** - Stack space usage.

---

### Two-Pointer Approach

This method uses two passes over the string with two pointer variables `left` and `right` to count open and close parentheses, optimizing both the forward and backward scans.

#### Code:
```cpp
class Solution {
public:
    int longestValidParentheses(string s) {
        int left = 0, right = 0, max_len = 0;
        
        // Left to right
        for (char c : s) {
            if (c == '(') {
                left++;
            } else {
                right++;
            }
            if (left == right) {
                max_len = max(max_len, 2 * right);
            } else if (right > left) {
                left = right = 0;
            }
        }
        
        // Reset and right to left
        left = right = 0;
        for (int i = s.length() - 1; i >= 0; --i) {
            if (s[i] == '(') {
                left++;
            } else {
                right++;
            }
            if (left == right) {
                max_len = max(max_len, 2 * left);
            } else if (left > right) {
                left = right = 0;
            }
        }
        
        return max_len;
    }
};
```

#### Time Complexity:
- **O(n)** - Two linear passes through the string.
- **Space Complexity: O(1)** - No extra space, just variables. 

This approach ensures constant space usage while effectively counting and matching parentheses from both directions.

