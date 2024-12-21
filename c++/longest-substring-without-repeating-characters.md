# [Leetcode 3: Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

## Approaches
1. [Brute Force](#1-brute-force)
2. [Sliding Window](#2-sliding-window)
3. [Optimized Sliding Window with Hash Map](#3-optimized-sliding-window-with-hash-map)

### 1. Brute Force

In the brute force approach, we examine every possible substring of the given string `s` to check if it contains all unique characters, updating the maximum length accordingly.

#### Intuition:
- Traverse the string with two pointers, considering every possible pair of start and end indices.
- For each pair, check the substring between these indices whether all characters are unique by using a set.
- If they are, calculate the length and update the maximum length accordingly.

#### Code:
```cpp
#include <unordered_set>
#include <string>
using namespace std;

int lengthOfLongestSubstring(string s) {
    int maxLength = 0;
    
    // Step through all possible starting points of substrings
    for (int i = 0; i < s.size(); ++i) {
        unordered_set<char> seenChars;
        
        // Explore each possible substring starting at position i
        for (int j = i; j < s.size(); ++j) {
            // Check if the character is already seen
            if (seenChars.find(s[j]) != seenChars.end()) {
                break;
            }
            seenChars.insert(s[j]);
            maxLength = max(maxLength, j - i + 1);
        }
    }
    return maxLength;
}
```

#### Complexity:
- **Time Complexity**: O(n^3) due to the two nested loops and set operations within.
- **Space Complexity**: O(n), to store the characters in the set.

### 2. Sliding Window

In the sliding window approach, maintain a window that represents a substring with all unique characters, and slide the window without resetting it completely.

#### Intuition:
- Use two pointers to represent the current window, start and end.
- As you iterate the string with the end pointer, add characters to a set until you hit a duplicate. 
- Once a duplicate is found, move the start pointer up to remove characters from the left of the window until there are no duplicates.
- During each step, update the longest found unique substring length.

#### Code:
```cpp
#include <unordered_set>
#include <string>
using namespace std;

int lengthOfLongestSubstring(string s) {
    int maxLength = 0;
    unordered_set<char> seenChars;
    int start = 0; // Start pointer of the sliding window

    for (int end = 0; end < s.size(); ++end) {
        // While the current character is a duplicate in the window
        while (seenChars.find(s[end]) != seenChars.end()) {
            seenChars.erase(s[start++]); // Remove starting character and move forward
        }
        seenChars.insert(s[end]);
        maxLength = max(maxLength, end - start + 1); // Update max length
    }
    return maxLength;
}
```

#### Complexity:
- **Time Complexity**: O(n), as each character is visited at most twice.
- **Space Complexity**: O(min(n, m)), where n is the string length and m is the character set size.

### 3. Optimized Sliding Window with Hash Map

Using a hash map, we can optimize further by keeping track of the last known index of each character.

#### Intuition:
- Utilize a hash map to track the latest index of each character in the string.
- Traverse through `s` with a single pointer (`end`).
  - Check if the current character has been seen and is within the current window.
  - If so, move the `start` pointer to the position right after the last occurrence of this character.
  - Calculate and update the longest unique substring length during each iteration.

#### Code:
```cpp
#include <unordered_map>
#include <string>
using namespace std;

int lengthOfLongestSubstring(string s) {
    unordered_map<char, int> lastSeenIndex;
    int maxLength = 0;
    int start = 0;

    for (int end = 0; end < s.size(); ++end) {
        // If we have seen the character and it is within the current window
        if (lastSeenIndex.find(s[end]) != lastSeenIndex.end() && lastSeenIndex[s[end]] >= start) {
            start = lastSeenIndex[s[end]] + 1; // Move start right after last seen occurrence
        }
        lastSeenIndex[s[end]] = end;
        maxLength = max(maxLength, end - start + 1); // Update max length
    }
    return maxLength;
}
```

#### Complexity:
- **Time Complexity**: O(n), where n is the string length. Each character is processed once.
- **Space Complexity**: O(min(n, m)), where n is the string length and m is the character set size.

