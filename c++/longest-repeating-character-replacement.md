# [LeetCode 424: Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)

- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Sliding Window with Frequency Array](#approach-2-sliding-window-with-frequency-array)

---

## Approach 1: Brute Force

### Intuition:
The brute force approach would involve examining every possible substring of the given string `s` and checking if we can make all the characters in that substring the same by replacing at most `k` characters. This approach, while straightforward, is very inefficient.

### Steps:
1. Iterate over each character in the string `s` as the starting point.
2. For each starting point, iterate over every possible ending point.
3. Count the frequency of each character in the current substring.
4. Determine if it is possible to make the substring consist of the same character by replacing at most `k` characters.

### Code:
```cpp
#include <iostream>
#include <unordered_map>
#include <string>
using namespace std;

int characterReplacement(string s, int k) {
    int n = s.length();
    int max_length = 0;
    
    // Try every possible starting point
    for (int start = 0; start < n; ++start) {
        // Map to track character frequencies
        unordered_map<char, int> count_map;
        int max_count = 0;
        
        // Extend the window from the current starting point
        for (int end = start; end < n; ++end) {
            count_map[s[end]]++;
            max_count = max(max_count, count_map[s[end]]);
            
            // Calculate the number of replacements needed to make the current substring valid
            int current_length = end - start + 1;
            int replacements_needed = current_length - max_count;
            
            // Check if the current substring can be made valid with at most k replacements
            if (replacements_needed <= k) {
                max_length = max(max_length, current_length);
            } else {
                break; // Early exit as further increase won't help
            }
        }
    }
    return max_length;
}
```

### Complexity Analysis:
- **Time Complexity**: O(n^2), where n is the length of the string `s`. We are examining each possible substring.
- **Space Complexity**: O(1) (ignoring the map size as it has at most 26 keys).

---

## Approach 2: Sliding Window with Frequency Array

### Intuition:
A more efficient approach leverages a sliding window technique. We maintain a window [start, end] and try to increase the window's size as much as possible while ensuring that, with at most `k` replacements, the characters in the window can be the same.

### Steps:
1. Use a character frequency array to count occurrences of characters within the current window.
2. Keep track of the character with the maximum frequency in the current window.
3. If the window length minus the maximum frequency is greater than `k`, shrink the window from the left.
4. Calculate the maximum length of the window seen so far.

### Code:
```cpp
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;

int characterReplacement(string s, int k) {
    vector<int> count(26, 0);
    int max_count = 0;
    int start = 0;
    int max_length = 0;

    for (int end = 0; end < s.size(); ++end) {
        max_count = max(max_count, ++count[s[end] - 'A']);
        
        // If replacements needed exceed k, shrink the window from the left
        while ((end - start + 1) - max_count > k) {
            count[s[start] - 'A']--;
            start++;
        }
        
        max_length = max(max_length, end - start + 1);
    }
    return max_length;
}
```

### Complexity Analysis:
- **Time Complexity**: O(n), where n is the length of the string `s`. We process each character in the string once.
- **Space Complexity**: O(1). The frequency array size is constant (26 letters of the alphabet).

This sliding window solution is optimal and provides a significant improvement over the brute force method.

