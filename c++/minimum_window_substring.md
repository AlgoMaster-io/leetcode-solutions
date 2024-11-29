# [76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

## Approach 1: Brute Force (Basic)

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(1)
#include <unordered_map>
#include <string>
#include <climits>

using namespace std;

class Solution {
public:
    string minWindow(string s, string t) {
        if (t.length() > s.length()) return "";

        string result = "";
        int minLength = INT_MAX;

        // Iterate over all substrings
        for (int i = 0; i < s.length(); i++) {
            for (int j = i; j < s.length(); j++) {
                string sub = s.substr(i, j - i + 1);

                if (containsAll(sub, t)) {
                    if (sub.length() < minLength) {
                        minLength = sub.length();
                        result = sub;
                    }
                }
            }
        }

        return result;
    }

private:
    bool containsAll(const string& sub, const string& t) {
        unordered_map<char, int> map;

        for (char c : t) {
            map[c]++;
        }

        for (char c : sub) {
            if (map.count(c)) {
                map[c]--;
                if (map[c] == 0) {
                    map.erase(c);
                }
            }
        }

        return map.empty();
    }
};
```

## Approach 2: Sliding Window with Frequency Count (Optimal)

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
#include <unordered_map>
#include <string>
#include <climits>

using namespace std;

class Solution {
public:
    string minWindow(string s, string t) {
        if (t.length() > s.length()) return "";

        unordered_map<char, int> tFreq;
        for (char c : t) {
            tFreq[c]++;
        }

        unordered_map<char, int> windowFreq;
        int left = 0, right = 0, matched = 0;
        int minLength = INT_MAX;
        int start = 0;

        while (right < s.length()) {
            char c = s[right];
            windowFreq[c]++;

            if (tFreq.count(c) && windowFreq[c] == tFreq[c]) {
                matched++;
            }

            while (matched == tFreq.size()) {
                // Update the result if this window is smaller
                if (right - left + 1 < minLength) {
                    minLength = right - left + 1;
                    start = left;
                }

                // Shrink the window
                char leftChar = s[left];
                if (tFreq.count(leftChar) && windowFreq[leftChar] == tFreq[leftChar]) {
                    matched--;
                }
                windowFreq[leftChar]--;
                if (windowFreq[leftChar] == 0) {
                    windowFreq.erase(leftChar);
                }
                left++;
            }

            right++;
        }

        return minLength == INT_MAX ? "" : s.substr(start, minLength);
    }
};
```

