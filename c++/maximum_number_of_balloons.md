# [1189. Maximum Number of Balloons](https://leetcode.com/problems/maximum-number-of-balloons/)

## Approach 1: Frequency Count with HashMap

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)
#include <unordered_map>
#include <algorithm>

class Solution {
public:
    int maxNumberOfBalloons(const std::string& text) {
        std::unordered_map<char, int> countMap;

        // Count frequencies of each character in the text
        for (char c : text) {
            countMap[c]++;
        }

        // Check the minimum occurrences of characters required for "balloon"
        int b = countMap['b'];
        int a = countMap['a'];
        int l = countMap['l'] / 2; // 'l' appears twice in "balloon"
        int o = countMap['o'] / 2; // 'o' appears twice in "balloon"
        int n = countMap['n'];

        // Return the minimum number of complete "balloon" words
        return std::min({b, a, l, o, n});
    }
};
```

## Approach 2: Frequency Count with Array

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)
#include <string>
#include <algorithm>

class Solution {
public:
    int maxNumberOfBalloons(const std::string& text) {
        int charCount[26] = {0}; // Frequency array for all characters

        // Count frequencies of each character in the text
        for (char c : text) {
            charCount[c - 'a']++;
        }

        // Check the minimum occurrences of characters required for "balloon"
        int b = charCount['b' - 'a'];
        int a = charCount['a' - 'a'];
        int l = charCount['l' - 'a'] / 2; // 'l' appears twice in "balloon"
        int o = charCount['o' - 'a'] / 2; // 'o' appears twice in "balloon"
        int n = charCount['n' - 'a'];

        // Return the minimum number of complete "balloon" words
        return std::min({b, a, l, o, n});
    }
};
```

## Approach 3: Optimized Single Pass

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)
#include <string>
#include <algorithm>

class Solution {
public:
    int maxNumberOfBalloons(const std::string& text) {
        int counts[5] = {0}; // Indices: 0->b, 1->a, 2->l, 3->o, 4->n

        // Count only relevant characters
        for (char c : text) {
            switch (c) {
                case 'b': counts[0]++; break;
                case 'a': counts[1]++; break;
                case 'l': counts[2]++; break;
                case 'o': counts[3]++; break;
                case 'n': counts[4]++; break;
            }
        }

        // 'l' and 'o' need to be divided by 2 as they appear twice in "balloon"
        counts[2] /= 2;
        counts[3] /= 2;

        // Return the minimum count
        return *std::min_element(counts, counts + 5);
    }
};
```

