# [Problem: Leetcode 567 - Permutation in String](https://leetcode.com/problems/permutation-in-string/)

## Approaches:
- [Approach 1: Brute Force using String Permutations](#approach-1-brute-force-using-string-permutations)
- [Approach 2: Sliding Window with Array Comparison](#approach-2-sliding-window-with-array-comparison)
- [Approach 3: Optimized Sliding Window with Frequency Count](#approach-3-optimized-sliding-window-with-frequency-count)

### Approach 1: Brute Force using String Permutations

**Intuition:**

The simplest idea is to generate all permutations of the string `s1` and check if any of these permutations are a substring of `s2`. This leverages the definition of permutation itself but is not efficient for larger input sizes because the number of permutations grows factorially.

**Time Complexity:** O(n! * m), where n is the length of `s1` and m is the length of `s2`.

**Space Complexity:** O(n), for storing permutations.

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

bool checkInclusion(std::string s1, std::string s2) {
    // Generate all permutations of s1
    std::sort(s1.begin(), s1.end());
    do {
        // Check if current permutation is a substring of s2
        if(s2.find(s1) != std::string::npos) {
            return true;
        }
    } while(std::next_permutation(s1.begin(), s1.end()));
    return false;
}
```

### Approach 2: Sliding Window with Array Comparison

**Intuition:**

A more efficient strategy is to use a fixed-size sliding window of length `s1` on `s2` and compare if the character counts are equivalent. This is better than explicitly generating permutations.

**Steps:**
1. Initialize frequency count arrays for `s1` and the first window of `s2`.
2. Slide the window over `s2` by adjusting the counts for the incoming and outgoing characters.
3. Compare the frequency arrays.

**Time Complexity:** O(N + M), where N is the length of `s1` and M is the length of `s2`.

**Space Complexity:** O(1), because the frequency arrays are of constant size (26 for each letter).

```cpp
#include <iostream>
#include <string>
#include <vector>

bool checkInclusion(std::string s1, std::string s2) {
    if (s1.length() > s2.length()) return false;
    
    std::vector<int> s1Count(26, 0);
    std::vector<int> s2Count(26, 0);
    
    // Build the frequency count for s1 and the first window of s2
    for (int i = 0; i < s1.length(); ++i) {
        s1Count[s1[i] - 'a']++;
        s2Count[s2[i] - 'a']++;
    }
    
    // Check if the first window is a permutation
    if (s1Count == s2Count) return true;
    
    // Slide the window across s2
    for (int i = s1.length(); i < s2.length(); ++i) {
        // Add new character to current window
        s2Count[s2[i] - 'a']++;
        // Remove the character left out of window
        s2Count[s2[i - s1.length()] - 'a']--;
        
        // Compare the frequency arrays
        if (s1Count == s2Count) return true;
    }
    
    return false;
}
```

### Approach 3: Optimized Sliding Window with Frequency Count

**Intuition:**

This approach builds on Approach 2 but further optimizes the sliding window by using a single integer to keep track of the number of matches between the two frequency arrays, avoiding repeated array comparisons.

**Steps:**
1. Initialize frequency counts and a "match count" variable.
2. Slide the window over `s2`, updating the match count as you go.
3. If all matches are found, return true.

**Time Complexity:** O(N + M), where N is the length of `s1` and M is the length of `s2`.

**Space Complexity:** O(1), using constant additional space for frequency arrays and variables.

```cpp
#include <iostream>
#include <string>
#include <vector>

bool checkInclusion(std::string s1, std::string s2) {
    if (s1.length() > s2.length()) return false;

    std::vector<int> s1Count(26, 0);
    std::vector<int> s2Count(26, 0);
    int matches = 0;

    for (char c : s1) s1Count[c - 'a']++;

    for (int i = 0; i < s2.length(); ++i) {
        if (i < s1.length()) {
            s2Count[s2[i] - 'a']++;
            if (s2Count[s2[i] - 'a'] == s1Count[s2[i] - 'a']) matches++;
            else if (s2Count[s2[i] - 'a'] == s1Count[s2[i] - 'a'] + 1) matches--;
        } else {
            int addIdx = s2[i] - 'a';
            int removeIdx = s2[i - s1.length()] - 'a';

            s2Count[addIdx]++;
            if (s2Count[addIdx] == s1Count[addIdx]) matches++;
            else if (s2Count[addIdx] == s1Count[addIdx] + 1) matches--;

            s2Count[removeIdx]--;
            if (s2Count[removeIdx] == s1Count[removeIdx]) matches++;
            else if (s2Count[removeIdx] == s1Count[removeIdx] - 1) matches--;
        }

        if (matches == 26) return true;
    }

    return false;
}
```

In this final approach, we keep track of the character frequencies and efficiently update our matches with each slide step. This solution is particularly optimized for scenarios involving larger strings and character sets.

