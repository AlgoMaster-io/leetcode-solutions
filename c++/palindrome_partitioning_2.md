# 132. [Palindrome Partitioning II](https://leetcode.com/problems/palindrome-partitioning-ii/)

## Approach 1: Dynamic Programming with Palindrome Check

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(n^2)
#include <vector>
#include <string>
#include <algorithm>

class Solution {
public:
    int minCut(std::string s) {
        int n = s.length();
        std::vector<std::vector<bool>> isPalindrome(n, std::vector<bool>(n, false));

        // Fill the isPalindrome table
        for (int length = 1; length <= n; length++) {
            for (int i = 0; i <= n - length; i++) {
                int j = i + length - 1;
                if (s[i] == s[j]) {
                    isPalindrome[i][j] = length <= 2 || isPalindrome[i + 1][j - 1];
                }
            }
        }

        std::vector<int> cuts(n);
        for (int i = 0; i < n; i++) {
            if (isPalindrome[0][i]) {
                cuts[i] = 0;
            } else {
                cuts[i] = i;
                for (int j = 0; j < i; j++) {
                    if (isPalindrome[j + 1][i]) {
                        cuts[i] = std::min(cuts[i], cuts[j] + 1);
                    }
                }
            }
        }

        return cuts[n - 1];
    }
};
```

## Approach 2: Optimized with Less Space for Palindrome Check

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(n)
#include <vector>
#include <string>
#include <algorithm>

class Solution {
public:
    int minCut(std::string s) {
        int n = s.length();
        std::vector<int> cuts(n);
        std::vector<bool> isPalindrome(n, false);

        for (int i = 0; i < n; i++) {
            int minCuts = i;
            for (int j = 0; j <= i; j++) {
                if (s[j] == s[i] && (i - j <= 1 || isPalindrome[j + 1])) {
                    isPalindrome[j] = true;
                    minCuts = std::min(minCuts, j == 0 ? 0 : cuts[j - 1] + 1);
                } else {
                    isPalindrome[j] = false;
                }
            }
            cuts[i] = minCuts;
        }

        return cuts[n - 1];
    }
};
```

