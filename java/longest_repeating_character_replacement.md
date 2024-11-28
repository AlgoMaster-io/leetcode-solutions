# [424. Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)

## Approach 1: Brute Force (Basic)

### Solution
```java
// Time Complexity: O(n^2)
// Space Complexity: O(1)
public class Solution {
    public int characterReplacement(String s, int k) {
        int maxLength = 0;

        // Iterate through every possible substring
        for (int i = 0; i < s.length(); i++) {
            int[] freq = new int[26];
            int maxFreq = 0;

            for (int j = i; j < s.length(); j++) {
                freq[s.charAt(j) - 'A']++;
                maxFreq = Math.max(maxFreq, freq[s.charAt(j) - 'A']);

                // Check if the current substring can be made uniform
                if ((j - i + 1) - maxFreq <= k) {
                    maxLength = Math.max(maxLength, j - i + 1);
                }
            }
        }

        return maxLength;
    }
}
```

## Approach 2: Sliding Window (Optimal)

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int characterReplacement(String s, int k) {
        int[] freq = new int[26];
        int maxFreq = 0, maxLength = 0;
        int left = 0;

        // Sliding window
        for (int right = 0; right < s.length(); right++) {
            freq[s.charAt(right) - 'A']++;
            maxFreq = Math.max(maxFreq, freq[s.charAt(right) - 'A']);

            // If the remaining characters exceed k, shrink the window
            while ((right - left + 1) - maxFreq > k) {
                freq[s.charAt(left) - 'A']--;
                left++;
            }

            // Update maxLength
            maxLength = Math.max(maxLength, right - left + 1);
        }

        return maxLength;
    }
}
```