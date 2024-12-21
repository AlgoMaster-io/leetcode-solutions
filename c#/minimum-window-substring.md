# [Leetcode 76: Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

## Approaches

- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Sliding Window with HashMap](#approach-2-sliding-window-with-hashmap)

---

## Approach 1: Brute Force

### Intuition

The brute force solution involves checking all possible substrings of `s` to see if they contain all characters of `t`. For each substring, we count characters and compare it to the character counts required by `t`. This approach is simple to implement but inefficient, especially for long strings, because it involves checking a large number of substrings.

### Detailed Steps

1. Iterate over all possible starting indices of substrings in `s`.
2. For each starting index, iterate over all possible ending indices to form substrings.
3. For each substring, check if it contains all characters from `t`.
4. Keep track of the minimum length substring that meets the criteria.

### C# Code

```csharp
public class Solution {
    public string MinWindow(string s, string t) {
        if (s.Length == 0 || t.Length == 0) return "";
        
        Dictionary<char, int> tCount = new Dictionary<char, int>();
        
        // Count characters in t
        foreach (char c in t) {
            if (tCount.ContainsKey(c)) {
                tCount[c]++;
            } else {
                tCount[c] = 1;
            }
        }
        
        int minLength = Int32.MaxValue;
        string result = "";
        
        for (int start = 0; start < s.Length; ++start) {
            // Initialize window map
            Dictionary<char, int> windowCounts = new Dictionary<char, int>();
            
            for (int end = start; end < s.Length; ++end) {
                char endChar = s[end];
                if (windowCounts.ContainsKey(endChar)) {
                    windowCounts[endChar]++;
                } else {
                    windowCounts[endChar] = 1;
                }
                
                // Check if current window contains all characters in t
                bool valid = true;
                foreach (var key in tCount.Keys) {
                    if (!windowCounts.ContainsKey(key) || windowCounts[key] < tCount[key]) {
                        valid = false;
                        break;
                    }
                }
                
                // Update minimum length substring
                if (valid) {
                    int windowLength = end - start + 1;
                    if (windowLength < minLength) {
                        minLength = windowLength;
                        result = s.Substring(start, windowLength);
                    }
                    break;
                }
            }
        }
        
        return result;
    }
}
```

### Time Complexity

- **Time complexity**: O(n^2 * m), where n is the length of the string `s` and m is the length of the string `t`. Checking each substring's validity involves counting operations which may pass over `t` multiple times.
- **Space complexity**: O(m), to store character counts in `t`.

---

## Approach 2: Sliding Window with HashMap

### Intuition

The sliding window technique can be optimal when you need to reduce the time complexity in problems regarding substrings containing elements from another set. We maintain a window using two pointers which expand and contract in a way that the window contains all the required characters of `t` minimally. We use hash maps to track necessary and current counts of characters, efficiently checking when the window satisfies the condition.

### Detailed Steps

1. Use two pointers: `left` and `right`, which denote the window's boundaries.
2. Expand `right` to find a valid window containing all characters from `t`.
3. Once a valid window is found, contract the window from the `left`.
4. Keep track of the minimum window length that satisfies the condition.
5. Repeat this until the end of the string `s`.

### C# Code

```csharp
public class Solution {
    public string MinWindow(string s, string t) {
        if (s.Length == 0 || t.Length == 0) return "";
        
        Dictionary<char, int> tCount = new Dictionary<char, int>();
        foreach (char c in t) {
            if (tCount.ContainsKey(c)) {
                tCount[c]++;
            } else {
                tCount[c] = 1;
            }
        }
        
        Dictionary<char, int> windowCounts = new Dictionary<char, int>();
        int left = 0, right = 0, formed = 0;
        int required = tCount.Keys.Count;
        int minLength = Int32.MaxValue;
        int[] minWindow = {-1, 0, 0}; // length, left, right
        
        while (right < s.Length) {
            char c = s[right];
            if (windowCounts.ContainsKey(c)) {
                windowCounts[c]++;
            } else {
                windowCounts[c] = 1;
            }
            
            // Check if a complete requirement is met
            if (tCount.ContainsKey(c) && windowCounts[c] == tCount[c]) {
                formed++;
            }
            
            // Try to contract from left until the window is no longer valid
            while (left <= right && formed == required) {
                c = s[left];
                
                // Update our minimum result
                if (right - left + 1 < minLength) {
                    minLength = right - left + 1;
                    minWindow[0] = minLength;
                    minWindow[1] = left;
                    minWindow[2] = right;
                }
                
                // Remove character from left of window
                windowCounts[c]--;
                if (tCount.ContainsKey(c) && windowCounts[c] < tCount[c]) {
                    formed--;
                }
                
                left++;
            }
            
            right++;
        }
        
        return minWindow[0] == -1 ? "" : s.Substring(minWindow[1], minWindow[0]);
    }
}
```

### Time Complexity

- **Time complexity**: O(n + m), where n is the length of `s` and m is the length of `t`. Each character is iterated over twice at most.
- **Space complexity**: O(m + n), where we store counts of `s` and `t` in hash maps.

This approach is significantly more efficient by reducing the problem to linear time complexity with respect to the input size, leveraging the sliding window technique for optimal performance.

