# [Leetcode 424: Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)

Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Sliding Window](#approach-2-sliding-window)

## Approach 1: Brute Force

### Intuition:
The brute force approach involves examining every possible window in the string to determine the longest substring that can be obtained by replacing `k` existing characters. For each window, count the occurrences of each character and decide if it can be turned into a valid substring with replacements.

### Implementation:

```csharp
public class Solution {
    public int CharacterReplacement(string s, int k) {
        int maxLen = 0;
        
        // Check every possible starting point of the substring
        for (int i = 0; i < s.Length; i++) {
            int[] count = new int[26]; // To keep track of character counts
            int maxCount = 0; // Maximum frequency of a single character in the current window
            
            // Try to extend the window ending at every point j after i
            for (int j = i; j < s.Length; j++) {
                count[s[j] - 'A']++;
                maxCount = Math.Max(maxCount, count[s[j] - 'A']); // Update max count of any single character
                
                // If the window size minus the maxCount is less than or equal to k
                // We can replace the remaining characters to make the window valid
                if (j - i + 1 - maxCount <= k) {
                    maxLen = Math.Max(maxLen, j - i + 1);
                }
            }
        }
        
        return maxLen;
    }
}
```

### Complexity Analysis:
- **Time Complexity:** O(n^2), where n is the length of the string. We check each substring and for each of them, we count the characters.
- **Space Complexity:** O(1). We use a fixed-size array (26) to store character counts.

## Approach 2: Sliding Window

### Intuition:
The sliding window technique allows us to dynamically adjust the size of the current substring window without re-evaluating the entire string for every potential starting point. The main insight here is that we can maintain a count of the most frequent character in the current window and adjust the start of the window when the number of replacements exceeds `k`.

### Implementation:

```csharp
public class Solution {
    public int CharacterReplacement(string s, int k) {
        int[] count = new int[26]; // To keep track of character counts
        int left = 0, maxCount = 0, maxLength = 0;
        
        // Expand the window with end being the right boundary
        for (int right = 0; right < s.Length; right++) {
            // Increase the count of the current character
            count[s[right] - 'A']++;
            maxCount = Math.Max(maxCount, count[s[right] - 'A']); // Update max count of any single character
            
            // If too many characters need to be replaced, shrink from the left
            while (right - left + 1 - maxCount > k) {
                count[s[left] - 'A']--; // Decrease the count of the char going out of the window
                left++; // Move the window's start forward
            }
            
            maxLength = Math.Max(maxLength, right - left + 1); // Update the maximum length found
        }
        
        return maxLength;
    }
}
```

### Complexity Analysis:
- **Time Complexity:** O(n), where n is the length of the string. Each character is checked at most twice.
- **Space Complexity:** O(1). We use a fixed-size array (26) to store character counts.

