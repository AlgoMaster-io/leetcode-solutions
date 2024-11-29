# 132. [Palindrome Partitioning II](https://leetcode.com/problems/palindrome-partitioning-ii/)

## Approach 1: Dynamic Programming with Palindrome Check

### Solution
csharp
```csharp
// Time Complexity: O(n^2)
// Space Complexity: O(n^2)
public class Solution {
    public int MinCut(string s) {
        int n = s.Length;
        bool[,] isPalindrome = new bool[n, n];
        
        // Fill the isPalindrome table
        for (int length = 1; length <= n; length++) {
            for (int i = 0; i <= n - length; i++) {
                int j = i + length - 1;
                if (s[i] == s[j]) {
                    isPalindrome[i, j] = length <= 2 || isPalindrome[i + 1, j - 1];
                }
            }
        }
        
        int[] cuts = new int[n];
        for (int i = 0; i < n; i++) {
            if (isPalindrome[0, i]) {
                cuts[i] = 0;
            } else {
                cuts[i] = i;
                for (int j = 0; j < i; j++) {
                    if (isPalindrome[j + 1, i]) {
                        cuts[i] = Math.Min(cuts[i], cuts[j] + 1);
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
csharp
```csharp
// Time Complexity: O(n^2)
// Space Complexity: O(n)
public class Solution {
    public int MinCut(string s) {
        int n = s.Length;
        int[] cuts = new int[n];
        bool[] isPalindrome = new bool[n];
        
        for (int i = 0; i < n; i++) {
            int minCuts = i;
            for (int j = 0; j <= i; j++) {
                if (s[j] == s[i] && (i - j <= 1 || isPalindrome[j + 1])) {
                    isPalindrome[j] = true;
                    minCuts = Math.Min(minCuts, j == 0 ? 0 : cuts[j - 1] + 1);
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

