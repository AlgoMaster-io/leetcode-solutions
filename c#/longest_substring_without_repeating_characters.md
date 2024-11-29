# [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

## Approach 1: Brute Force (Basic)

### Solution
csharp
```csharp
// Time Complexity: O(n^2)
// Space Complexity: O(n)
using System;
using System.Collections.Generic;

public class Solution {
    public int LengthOfLongestSubstring(string s) {
        int maxLength = 0;

        // Check every possible substring
        for (int i = 0; i < s.Length; i++) {
            HashSet<char> seen = new HashSet<char>();
            int length = 0;

            for (int j = i; j < s.Length; j++) {
                if (seen.Contains(s[j])) {
                    break; // Break if a duplicate character is found
                }
                seen.Add(s[j]);
                length++;
                maxLength = Math.Max(maxLength, length); // Update maxLength
            }
        }

        return maxLength;
    }
}
```

## Approach 2: Sliding Window with HashSet

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
using System;
using System.Collections.Generic;

public class Solution {
    public int LengthOfLongestSubstring(string s) {
        HashSet<char> set = new HashSet<char>();
        int maxLength = 0;
        int left = 0;

        // Sliding window
        for (int right = 0; right < s.Length; right++) {
            while (set.Contains(s[right])) {
                set.Remove(s[left]); // Shrink the window
                left++;
            }
            set.Add(s[right]); // Expand the window
            maxLength = Math.Max(maxLength, right - left + 1); // Update maxLength
        }

        return maxLength;
    }
}
```

## Approach 3: Sliding Window with HashMap (Optimal)

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
using System;
using System.Collections.Generic;

public class Solution {
    public int LengthOfLongestSubstring(string s) {
        Dictionary<char, int> map = new Dictionary<char, int>();
        int maxLength = 0;
        int left = 0;

        // Sliding window
        for (int right = 0; right < s.Length; right++) {
            char currentChar = s[right];

            // If the character is already in the map, update the left pointer
            if (map.ContainsKey(currentChar)) {
                left = Math.Max(left, map[currentChar] + 1);
            }

            // Update the character's last index in the map
            map[currentChar] = right;

            // Update maxLength
            maxLength = Math.Max(maxLength, right - left + 1);
        }

        return maxLength;
    }
}
```

