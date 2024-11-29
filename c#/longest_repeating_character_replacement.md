# [424. Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)

## Approach 1: Brute Force (Basic)

### Solution
csharp
```csharp
// Time Complexity: O(n^2)
// Space Complexity: O(1)
public class Solution {
    public int CharacterReplacement(string s, int k) {
        int maxLength = 0;

        // Iterate through every possible substring
        for (int i = 0; i < s.Length; i++) {
            int[] freq = new int[26];
            int maxFreq = 0;

            for (int j = i; j < s.Length; j++) {
                freq[s[j] - 'A']++;
                maxFreq = Math.Max(maxFreq, freq[s[j] - 'A']);

                // Check if the current substring can be made uniform
                if ((j - i + 1) - maxFreq <= k) {
                    maxLength = Math.Max(maxLength, j - i + 1);
                }
            }
        }

        return maxLength;
    }
}
```

## Approach 2: Sliding Window (Optimal)

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int CharacterReplacement(string s, int k) {
        int[] freq = new int[26];
        int maxFreq = 0, maxLength = 0;
        int left = 0;

        // Sliding window
        for (int right = 0; right < s.Length; right++) {
            freq[s[right] - 'A']++;
            maxFreq = Math.Max(maxFreq, freq[s[right] - 'A']);

            // If the remaining characters exceed k, shrink the window
            while ((right - left + 1) - maxFreq > k) {
                freq[s[left] - 'A']--;
                left++;
            }

            // Update maxLength
            maxLength = Math.Max(maxLength, right - left + 1);
        }

        return maxLength;
    }
}
```

