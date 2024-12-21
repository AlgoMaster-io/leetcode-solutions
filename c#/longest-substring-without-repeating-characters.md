# [Leetcode Problem 3: Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Sliding Window Approach](#sliding-window-approach)

---

### Brute Force Approach

In this approach, we simply try to find all substrings of the given string and for each substring, we check if it contains all unique characters. If it does, we then check if this substring is the longest one encountered so far.

#### Intuition:
The idea is to enumerate all possible substrings and check each one for uniqueness of characters. This is not efficient due to the high complexity but serves as a good starting point.

#### Time Complexity:
- O(n^3): Generating all substrings takes O(n^2) and checking each one for unique characters takes O(n).

#### Space Complexity:
- O(min(n, m)): We use a set to store the unique characters of the substring, where `m` is the character set size.

```csharp
public class Solution {
    public int LengthOfLongestSubstring(string s) {
        if (string.IsNullOrEmpty(s)) {
            return 0;
        }
        
        int maxLength = 0;
        
        for (int i = 0; i < s.Length; i++) {
            // HashSet to check unique characters in a substring
            HashSet<char> seenCharacters = new HashSet<char>();
            
            for (int j = i; j < s.Length; j++) {
                // If character is already in the set, break the loop
                if (seenCharacters.Contains(s[j])) {
                    break;
                }
                // Add character to the set
                seenCharacters.Add(s[j]);
                // Update max length
                maxLength = Math.Max(maxLength, j - i + 1);
            }
        }
        
        return maxLength;
    }
}
```

---

### Sliding Window Approach

The sliding window approach optimizes the process using a two-pointer technique that allows us to move a window across the string, expanding and contracting it to ensure we maintain the property of a substring without repeating characters.

#### Intuition:
- Utilize two pointers `i` and `j` to represent the current window. 
- Move `j` to expand the window until a repeated character is found.
- Move `i` to contract from the left until removing the repeated character.
- Keep track of the maximum length observed during these adjustments.

#### Time Complexity:
- O(n): Each character is processed at most twice, once through `j` and once through `i`.

#### Space Complexity:
- O(min(n, m)): Utilizes a dictionary to store and check for last seen positions of characters, where `m` is the character set size.

```csharp
public class Solution {
    public int LengthOfLongestSubstring(string s) {
        if (string.IsNullOrEmpty(s)) {
            return 0;
        }
        
        Dictionary<char, int> lastSeenIndex = new Dictionary<char, int>();
        int maxLength = 0;
        int i = 0; // Left pointer
        
        for (int j = 0; j < s.Length; j++) { // Right pointer
            char currentChar = s[j];
            
            // If character has already been seen, update the left pointer `i`
            if (lastSeenIndex.ContainsKey(currentChar)) {
                // Ensure the invariant that i moves forward
                i = Math.Max(lastSeenIndex[currentChar] + 1, i);
            }
            
            // Update max length
            maxLength = Math.Max(maxLength, j - i + 1);
            
            // Update last seen index of the current character
            lastSeenIndex[currentChar] = j;
        }
        
        return maxLength;
    }
}
```

---

