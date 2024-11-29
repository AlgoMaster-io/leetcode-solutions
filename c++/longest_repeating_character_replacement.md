# [424. Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)

## Approach 1: Brute Force (Basic)

### Solution
cpp
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(1)
#include <vector>
#include <algorithm>

class Solution {
public:
    int characterReplacement(std::string s, int k) {
        int maxLength = 0;
        
        // Iterate through every possible substring
        for (int i = 0; i < s.length(); i++) {
            std::vector<int> freq(26, 0);
            int maxFreq = 0;
            
            for (int j = i; j < s.length(); j++) {
                freq[s[j] - 'A']++;
                maxFreq = std::max(maxFreq, freq[s[j] - 'A']);
                
                // Check if the current substring can be made uniform
                if ((j - i + 1) - maxFreq <= k) {
                    maxLength = std::max(maxLength, j - i + 1);
                }
            }
        }
        
        return maxLength;
    }
};
```

## Approach 2: Sliding Window (Optimal)

### Solution
cpp
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)
#include <vector>
#include <algorithm>

class Solution {
public:
    int characterReplacement(std::string s, int k) {
        std::vector<int> freq(26, 0);
        int maxFreq = 0, maxLength = 0;
        int left = 0;
        
        // Sliding window
        for (int right = 0; right < s.length(); right++) {
            freq[s[right] - 'A']++;
            maxFreq = std::max(maxFreq, freq[s[right] - 'A']);
            
            // If the remaining characters exceed k, shrink the window
            while ((right - left + 1) - maxFreq > k) {
                freq[s[left] - 'A']--;
                left++;
            }
            
            // Update maxLength
            maxLength = std::max(maxLength, right - left + 1);
        }
        
        return maxLength;
    }
};
```

