# [567. Permutation in String](https://leetcode.com/problems/permutation-in-string/)

## Approach 1: Brute Force (Basic)

### Solution
csharp
```csharp
// Time Complexity: O(n * m), where n is the length of s1 and m is the length of s2
// Space Complexity: O(n)
using System;
using System.Linq;

public class Solution {
    public bool CheckInclusion(string s1, string s2) {
        int n = s1.Length, m = s2.Length;

        if (n > m) return false;

        // Sort s1
        char[] s1Arr = s1.ToCharArray();
        Array.Sort(s1Arr);
        string sortedS1 = new string(s1Arr);

        // Check every substring of length n in s2
        for (int i = 0; i <= m - n; i++) {
            string sub = s2.Substring(i, n);
            char[] subArr = sub.ToCharArray();
            Array.Sort(subArr);

            if (sortedS1.Equals(new string(subArr))) {
                return true;
            }
        }

        return false;
    }
}
```

## Approach 2: Sliding Window with Frequency Array (Optimal)

### Solution
csharp
```csharp
// Time Complexity: O(m), where m is the length of s2
// Space Complexity: O(1)
using System;

public class Solution {
    public bool CheckInclusion(string s1, string s2) {
        if (s1.Length > s2.Length) return false;

        int[] s1Freq = new int[26];
        int[] s2Freq = new int[26];

        // Frequency count for s1
        foreach (char c in s1) {
            s1Freq[c - 'a']++;
        }

        // Sliding window
        for (int i = 0; i < s2.Length; i++) {
            s2Freq[s2[i] - 'a']++;

            // Remove the leftmost character from the window
            if (i >= s1.Length) {
                s2Freq[s2[i - s1.Length] - 'a']--;
            }

            // Compare the frequency arrays
            if (s1Freq.SequenceEqual(s2Freq)) {
                return true;
            }
        }

        return false;
    }
}
```

