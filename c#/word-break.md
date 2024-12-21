[Leetcode Problem 139: Word Break](https://leetcode.com/problems/word-break/)

### Approaches
- [Brute Force Approach](#brute-force-approach)
- [Dynamic Programming Approach](#dynamic-programming-approach)
- [Memoization Approach](#memoization-approach)

---

#### Brute Force Approach

**Intuition**

The brute force solution tries every possible partition of the string `s` and checks if each partitioned substring is a valid word in the `wordDict`. This solution explores all possible ways to break the string, which might be inefficient for larger strings.

**Implementation**

```csharp
public class Solution {
    public bool WordBreak(string s, IList<string> wordDict) {
        return CanBreak(s, wordDict, 0);
    }
    
    private bool CanBreak(string s, IList<string> wordDict, int start) {
        if (start == s.Length) {
            return true; // Successfully reached the end of the string
        }
        
        for (int end = start + 1; end <= s.Length; end++) {
            // Check if the current substring is in the wordDictionary
            if (wordDict.Contains(s.Substring(start, end - start)) && CanBreak(s, wordDict, end)) {
                return true;
            }
        }
        
        return false;
    }
}
```

**Time Complexity:** \(O(2^n \cdot n)\) because we are exploring all subsets and each subset extraction takes `n` time.

**Space Complexity:** \(O(n)\) due to the recursion stack.

---

#### Dynamic Programming Approach

**Intuition**

This approach uses dynamic programming to store previously computed subproblems. The idea is to maintain a boolean array `dp` where `dp[i]` is `true` if the substring `s[0..i]` can be segmented into valid words from the dictionary. We iteratively build up the solution by solving smaller subproblems.

**Implementation**

```csharp
public class Solution {
    public bool WordBreak(string s, IList<string> wordDict) {
        HashSet<string> wordSet = new HashSet<string>(wordDict);
        bool[] dp = new bool[s.Length + 1];
        dp[0] = true; // base case, empty string
        
        for (int i = 1; i <= s.Length; i++) {
            for (int j = 0; j < i; j++) {
                // Check if s[0:j] can be segmented and s[j:i] is a valid word
                if (dp[j] && wordSet.Contains(s.Substring(j, i - j))) {
                    dp[i] = true;
                    break;
                }
            }
        }
        
        return dp[s.Length];
    }
}
```

**Time Complexity:** \(O(n^2)\), where `n` is the length of the string. For each position, we check all possible previous segments.

**Space Complexity:** \(O(n)\) to store the `dp` array.

---

#### Memoization Approach

**Intuition**

This approach is similar to the brute force solution but uses memoization to store the results of already solved subproblems and avoid redundant computations. We use a dictionary to cache the results of `CanBreak` for substrings.

**Implementation**

```csharp
public class Solution {
    public bool WordBreak(string s, IList<string> wordDict) {
        HashSet<string> wordSet = new HashSet<string>(wordDict);
        Dictionary<int, bool> memo = new Dictionary<int, bool>();
        return CanBreak(s, wordSet, 0, memo);
    }
    
    private bool CanBreak(string s, HashSet<string> wordSet, int start, Dictionary<int, bool> memo) {
        if (start == s.Length) {
            return true; // whole string has been segmented
        }
        
        if (memo.ContainsKey(start)) {
            return memo[start]; // return already computed result
        }
        
        for (int end = start + 1; end <= s.Length; end++) {
            if (wordSet.Contains(s.Substring(start, end - start)) && CanBreak(s, wordSet, end, memo)) {
                memo[start] = true;
                return true;
            }
        }
        
        memo[start] = false;
        return false;
    }
}
```

**Time Complexity:** \(O(n^2)\), since each substring checks against the dictionary once due to memoization.

**Space Complexity:** \(O(n)\), used by the memoization dictionary.

---

These approaches progressively optimize the naive solution, leveraging dynamic programming principles to efficiently solve the word break problem.

