# 214. [Shortest Palindrome](https://leetcode.com/problems/shortest-palindrome/)

## Approach 1: Brute Force Reverse and Check

### Solution
csharp
```csharp
// Time Complexity: O(n^2)
// Space Complexity: O(n)
public class Solution {
    public string ShortestPalindrome(string s) {
        // Edge case for empty string
        if (string.IsNullOrEmpty(s)) return s;
        
        // Reverse the string
        var sb = new StringBuilder(s);
        sb = sb.Remove(0, sb.Length).Append(s.Reverse().ToArray());
        
        // Try to find a palindrome by checking each position
        for (int i = 0; i < s.Length; i++) {
            // Check if the substring of s and reversed s is palindrome
            if (s.StartsWith(sb.ToString().Substring(i))) {
                return sb.ToString().Substring(0, i) + s;
            }
        }
        
        return ""; // This will never be executed
    }
}
```

## Approach 2: KMP Algorithm

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
public class Solution {
    public string ShortestPalindrome(string s) {
        string rev = new string(s.Reverse().ToArray());
        string combined = s + "#" + rev;

        // Compute KMP table
        int[] kmpTable = ComputeKMPTable(combined);
        
        // The palindrome length from using kmpTable
        int palindromeLength = kmpTable[combined.Length - 1];
        
        // Append the part missing to form a palindrome
        return rev.Substring(0, rev.Length - palindromeLength) + s;
    }

    private int[] ComputeKMPTable(string s) {
        int[] kmpTable = new int[s.Length];
        int j = 0;
        
        for (int i = 1; i < s.Length; i++) {
            while (j > 0 && s[i] != s[j]) {
                j = kmpTable[j - 1];
            }
            if (s[i] == s[j]) {
                j++;
            }
            kmpTable[i] = j;
        }
        
        return kmpTable;
    }
}
```

