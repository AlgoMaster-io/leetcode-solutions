# 76. [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

## Approach 1: Brute Force (Basic)

### Solution
csharp
```csharp
// Time Complexity: O(n^2)
// Space Complexity: O(1)
using System.Collections.Generic;

public class Solution {
    public string MinWindow(string s, string t) {
        if (t.Length > s.Length) return string.Empty;

        string result = string.Empty;
        int minLength = int.MaxValue;

        // Iterate over all substrings
        for (int i = 0; i < s.Length; i++) {
            for (int j = i; j < s.Length; j++) {
                string sub = s.Substring(i, j - i + 1);

                if (ContainsAll(sub, t)) {
                    if (sub.Length < minLength) {
                        minLength = sub.Length;
                        result = sub;
                    }
                }
            }
        }

        return result;
    }

    private bool ContainsAll(string sub, string t) {
        Dictionary<char, int> map = new Dictionary<char, int>();

        foreach (char c in t) {
            if (!map.ContainsKey(c)) {
                map[c] = 0;
            }
            map[c]++;
        }

        foreach (char c in sub) {
            if (map.ContainsKey(c)) {
                map[c]--;
                if (map[c] == 0) {
                    map.Remove(c);
                }
            }
        }

        return map.Count == 0;
    }
}
```

## Approach 2: Sliding Window with Frequency Count (Optimal)

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
using System.Collections.Generic;

public class Solution {
    public string MinWindow(string s, string t) {
        if (t.Length > s.Length) return string.Empty;

        Dictionary<char, int> tFreq = new Dictionary<char, int>();
        foreach (char c in t) {
            if (!tFreq.ContainsKey(c)) {
                tFreq[c] = 0;
            }
            tFreq[c]++;
        }

        Dictionary<char, int> windowFreq = new Dictionary<char, int>();
        int left = 0, right = 0, matched = 0;
        int minLength = int.MaxValue;
        int start = 0;

        while (right < s.Length) {
            char c = s[right];
            if (!windowFreq.ContainsKey(c)) {
                windowFreq[c] = 0;
            }
            windowFreq[c]++;
            
            if (tFreq.ContainsKey(c) && windowFreq[c] == tFreq[c]) {
                matched++;
            }

            while (matched == tFreq.Count) {
                // Update the result if this window is smaller
                if (right - left + 1 < minLength) {
                    minLength = right - left + 1;
                    start = left;
                }

                // Shrink the window
                char leftChar = s[left];
                if (tFreq.ContainsKey(leftChar) && windowFreq[leftChar] == tFreq[leftChar]) {
                    matched--;
                }
                windowFreq[leftChar]--;
                if (windowFreq[leftChar] == 0) {
                    windowFreq.Remove(leftChar);
                }
                left++;
            }

            right++;
        }

        return minLength == int.MaxValue ? string.Empty : s.Substring(start, minLength);
    }
}
```

