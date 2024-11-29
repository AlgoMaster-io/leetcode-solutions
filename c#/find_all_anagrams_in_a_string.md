# [438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

## Approach 1: Brute Force (Basic)

### Solution
```csharp
// Time Complexity: O(n * m), where n is the length of s and m is the length of p
// Space Complexity: O(m) for frequency array
using System;
using System.Collections.Generic;

public class Solution {
    public IList<int> FindAnagrams(string s, string p) {
        List<int> result = new List<int>();
        int[] pFreq = new int[26];

        // Frequency count for string p
        foreach (char c in p) {
            pFreq[c - 'a']++;
        }

        // Check every substring of length p in s
        for (int i = 0; i <= s.Length - p.Length; i++) {
            int[] sFreq = new int[26];

            // Count frequency of the current substring
            for (int j = i; j < i + p.Length; j++) {
                sFreq[s[j] - 'a']++;
            }

            // Compare frequencies
            if (AreArraysEqual(sFreq, pFreq)) {
                result.Add(i);
            }
        }

        return result;
    }

    private bool AreArraysEqual(int[] arr1, int[] arr2) {
        for (int i = 0; i < arr1.Length; i++) {
            if (arr1[i] != arr2[i]) {
                return false;
            }
        }
        return true;
    }
}
```

## Approach 2: Sliding Window with Frequency Array (Optimal)

### Solution
```csharp
// Time Complexity: O(n), where n is the length of s
// Space Complexity: O(1) (fixed frequency array of size 26)
using System;
using System.Collections.Generic;

public class Solution {
    public IList<int> FindAnagrams(string s, string p) {
        List<int> result = new List<int>();
        int[] pFreq = new int[26];
        int[] sFreq = new int[26];

        // Frequency count for string p
        foreach (char c in p) {
            pFreq[c - 'a']++;
        }

        int left = 0, right = 0;

        // Sliding window over s
        while (right < s.Length) {
            // Add the current character to the window
            sFreq[s[right] - 'a']++;

            // Check if the window size matches p
            if (right - left + 1 == p.Length) {
                // Compare frequencies
                if (AreArraysEqual(sFreq, pFreq)) {
                    result.Add(left);
                }

                // Remove the leftmost character from the window
                sFreq[s[left] - 'a']--;
                left++;
            }

            right++;
        }

        return result;
    }

    private bool AreArraysEqual(int[] arr1, int[] arr2) {
        for (int i = 0; i < arr1.Length; i++) {
            if (arr1[i] != arr2[i]) {
                return false;
            }
        }
        return true;
    }
}
```

