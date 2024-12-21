# [Leetcode 438: Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Sliding Window With Hashmap](#approach-2-sliding-window-with-hashmap)
- [Approach 3: Optimized Sliding Window with Constant Space](#approach-3-optimized-sliding-window-with-constant-space)

---

## Approach 1: Brute Force

### Intuition
The simplest approach is to consider all possible substrings of `s` with a length equal to `p`, then check if any of those substrings is an anagram of `p`. An anagram will have the same frequency of each character as the original string.

### Code
```cpp
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        vector<int> result;
        if(s.size() < p.size()) return result;
        
        vector<int> pCount(26, 0);
        // Count frequency of each character in p
        for(char c : p) {
            pCount[c - 'a']++;
        }
        
        // Sliding over the s, considering every substring of length equal to p 
        for(int i = 0; i <= s.size() - p.size(); ++i) {
            vector<int> sCount(26, 0);
            for(int j = 0; j < p.size(); ++j) {
                sCount[s[i+j] - 'a']++;
            }
            if(sCount == pCount) {
                result.push_back(i);
            }
        }
        
        return result;
    }
};
```

### Complexity
- **Time Complexity:** O((n - m + 1) * m), where n = length of string s, m = length of string p. We potentially have to check each substring of length m in s.
- **Space Complexity:** O(26) = O(1), for the frequency counts.

---

## Approach 2: Sliding Window With Hashmap

### Intuition
Leverage the sliding window approach with two hashmaps. Maintain a sliding window of size equal to `p` over `s`. Instead of recalculating the frequency from scratch for every window, we adjust by removing the outgoing character and adding the incoming character. This minimizes redundant calculations and improves efficiency.

### Code
```cpp
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        vector<int> result;
        if(s.size() < p.size()) return result;

        vector<int> pCount(26, 0), sCount(26, 0);
        for(char c : p) {
            pCount[c - 'a']++;
        }

        // Initial window
        for(int i = 0; i < p.size(); ++i) {
            sCount[s[i] - 'a']++;
        }

        if(sCount == pCount) result.push_back(0);

        for(int i = p.size(); i < s.size(); ++i) {
            // Sliding the window forward
            sCount[s[i] - 'a']++;
            sCount[s[i - p.size()] - 'a']--;

            if(sCount == pCount) result.push_back(i - p.size() + 1);
        }
        
        return result;
    }
};
```

### Complexity
- **Time Complexity:** O(n), where n = length of string s. We iterate over s once, processing each character.
- **Space Complexity:** O(1), since both `pCount` and `sCount` have a fixed size of 26.

---

## Approach 3: Optimized Sliding Window with Constant Space

### Intuition
Further optimize the sliding window approach by keeping track of only the count difference. By using a single array to manage the character counts and a counter for different chars, we optimize both time and space.

### Code
```cpp
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        vector<int> result;
        if(s.size() < p.size()) return result;

        vector<int> count(26, 0);
        for(char c : p) {
            count[c - 'a']++;
        }

        int left = 0, right = 0, difference = p.size();

        while(right < s.size()) {
            // Decrease difference when a matching character from p is found
            if(count[s[right] - 'a'] >= 1) {
                difference--;
            }
            count[s[right] - 'a']--;
            right++;

            // Once the window size equals p's length
            if(difference == 0) {
                result.push_back(left); // All characters match
            }

            // When exceeding the window size, slide the left pointer forward
            if(right - left == p.size()) {
                if(count[s[left] - 'a'] >= 0) {
                    difference++;
                }
                count[s[left] - 'a']++;
                left++;
            }
        }
        
        return result;
    }
};
```

### Complexity
- **Time Complexity:** O(n), where n = length of string s. We make a single pass over the data.
- **Space Complexity:** O(1), as we use a fixed-size count array.

