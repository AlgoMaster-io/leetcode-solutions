# [Leetcode 76: Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

## Approaches

1. [Sliding Window - Brute Force](#approach1)
2. [Sliding Window - Optimized with HashMaps](#approach2)

---

### Approach 1: Sliding Window - Brute Force
#### Intuition
This approach involves considering every possible substring of the input string `s` and checking if it contains all the characters of `t`. If it does, we compare its length to the current minimum and update accordingly. Although this method is intuitive, it is highly inefficient given its time complexity.

#### Steps
1. Generate all possible substrings of `s`.
2. For each substring, check if it contains all characters of `t` by using a frequency counter.
3. Keep track of the minimum-length substring which contains all the characters.

#### Time Complexity
- **Time:** O((s.length)^2 * t.length)
- **Space:** O(t.length) for the frequency counter.

```cpp
class Solution {
public:
    string minWindow(string s, string t) {
        if (s.empty() || t.empty()) return "";
        int minLength = INT_MAX;
        string result = "";

        for (int start = 0; start < s.length(); ++start) {
            unordered_map<char, int> map;
            for (char c : t) map[c]++;
            int end = start;
            while (end < s.length()) {
                if (map.find(s[end]) != map.end()) {
                    map[s[end]]--;
                    if (map[s[end]] == 0) map.erase(s[end]);
                }
                if (map.empty()) {
                    if (end - start + 1 < minLength) {
                        minLength = end - start + 1;
                        result = s.substr(start, minLength);
                    }
                    break; // try to move start to find shorter valid window
                }
                ++end;
            }
        }
        return result;
    }
};
```

---

### Approach 2: Sliding Window - Optimized with HashMaps
#### Intuition
This approach uses a sliding window technique with two pointers and a hash map to keep track of the characters in `t` that need to be covered. As we incrementally build the window on the string `s`, we expand to the right until the window contains all characters of `t`, then we shrink from the left to find the minimum window.

#### Steps
1. Use hash maps to store the frequency of characters required and the frequency in the current window of `s`.
2. Use two pointers, `left` and `right`, to denote the current window of examination in `s`.
3. Expand the `right` pointer to increase the window until it contains all characters of `t`.
4. Contract the `left` pointer to minimize the window size while still containing all characters of `t`.

#### Time Complexity
- **Time:** O(s.length + t.length), as each character is visited at most twice.
- **Space:** O(t.length) for the hash map of `t`'s characters.

```cpp
class Solution {
public:
    string minWindow(string s, string t) {
        if (s.length() < t.length()) return "";
        
        // Frequency maps for characters in t and current window
        unordered_map<char, int> tFreq, windowFreq;
        
        // Fill the t frequency map
        for (char c : t) tFreq[c]++;
        
        int start = 0, minLen = INT_MAX, count = 0;
        int left = 0;

        for (int right = 0; right < s.length(); ++right) {
            char c = s[right];
            // Increment the frequency of the current character in the window
            windowFreq[c]++;
            
            // Only increment the count if the window frequency for this character doesn't exceed the required frequency
            if (tFreq.count(c) && windowFreq[c] <= tFreq[c]) count++;
            
            // When all characters are matched, try to shrink the window
            while (count == t.length()) {
                if (right - left + 1 < minLen) {
                    minLen = right - left + 1;
                    start = left;
                }
                
                // Try to reduce the window size from the left
                char startChar = s[left];
                if (tFreq.count(startChar)) {
                    windowFreq[startChar]--;
                    if (windowFreq[startChar] < tFreq[startChar]) count--;
                }
                left++;
            }
        }
        return minLen == INT_MAX ? "" : s.substr(start, minLen);
    }
};
```

This optimized algorithm efficiently finds the minimum window substring in a linear time frame by leveraging two hash maps and a sliding window approach.

