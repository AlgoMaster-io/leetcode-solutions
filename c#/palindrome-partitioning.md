# [Leetcode 131: Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)

## Approaches:

1. [Backtracking Approach](#backtracking-approach)
2. [Dynamic Programming Precomputation + Backtracking](#dynamic-programming-precomputation-backtracking)

### Backtracking Approach

**Intuition:**

The basic idea is to use backtracking to partition the string into all possible palindromic substrings. For each substring that is a palindrome, recursively continue to partition the remaining substring.

The approach involves:
- Creating a recursive function that explores all the substrings starting from the current position.
- Checking if a substring is a palindrome, and if so, adding it to the current partitioning and continuing the search.
- Backtracking once a valid partitioning is complete and trying another possibility.

**C# Code:**

```csharp
public IList<IList<string>> Partition(string s) {
    var result = new List<IList<string>>();
    var currentPartition = new List<string>();
    Backtrack(s, 0, currentPartition, result);
    return result;
}

private void Backtrack(string s, int start, List<string> currentPartition, IList<IList<string>> result) {
    if (start == s.Length) {
        result.Add(new List<string>(currentPartition));
        return;
    }
    
    for (int end = start; end < s.Length; end++) {
        // Check if the substring from start to end is a palindrome
        if (IsPalindrome(s, start, end)) {
            currentPartition.Add(s.Substring(start, end - start + 1));
            // Explore further partitions with this palindromic substring
            Backtrack(s, end + 1, currentPartition, result);
            // Backtrack by removing the last added substring
            currentPartition.RemoveAt(currentPartition.Count - 1);
        }
    }
}

private bool IsPalindrome(string s, int left, int right) {
    while (left < right) {
        if (s[left] != s[right]) return false;
        left++;
        right--;
    }
    return true;
}
```

**Complexity Analysis:**

- Time Complexity: O(N * 2^N), where N is the length of the string. For each character, we decide whether to start a new partition or not.
- Space Complexity: O(N) for the recursion stack and space used to store current partitions.

### Dynamic Programming Precomputation + Backtracking

**Intuition:**

To optimize our palindrome checks, we can precompute which substrings of the string are palindromes using dynamic programming and then use this information during backtracking.

The approach involves:
- Using a 2D DP table to store boolean values indicating whether a substring `[i, j]` is a palindrome.
- Filling this table before backtracking so that each palindrome check is simply a lookup.
- Proceed with the backtracking using the precomputed DP table.

**C# Code:**

```csharp
public IList<IList<string>> Partition(string s) {
    var result = new List<IList<string>>();
    var currentPartition = new List<string>();
    int n = s.Length;
    
    // DP table to store palindrome status
    bool[,] dp = new bool[n,n];
    
    // Fill DP table
    for (int right = 0; right < n; right++) {
        for (int left = 0; left <= right; left++) {
            // A substring is a palindrome if the current characters are equal
            // and the remaining substring is a palindrome
            if (s[left] == s[right] && (right - left <= 2 || dp[left + 1, right - 1])) {
                dp[left, right] = true;
            }
        }
    }
    
    // Backtrack using the DP table
    Backtrack(s, 0, currentPartition, result, dp);
    
    return result;
}

private void Backtrack(string s, int start, List<string> currentPartition, IList<IList<string>> result, bool[,] dp) {
    if (start == s.Length) {
        result.Add(new List<string>(currentPartition));
        return;
    }
    
    for (int end = start; end < s.Length; end++) {
        // Use the precomputed DP table to check palindrome
        if (dp[start, end]) {
            currentPartition.Add(s.Substring(start, end - start + 1));
            Backtrack(s, end + 1, currentPartition, result, dp);
            currentPartition.RemoveAt(currentPartition.Count - 1);
        }
    }
}
```

**Complexity Analysis:**

- Time Complexity: O(N^2 + N * 2^N). Filling the DP table takes O(N^2), and the backtracking part is O(N * 2^N).
- Space Complexity: O(N^2) for the DP table and O(N) for the recursion stack.

Each solution builds upon the previous one by optimizing the palindrome checking process, enhancing the overall efficiency of the algorithm.

