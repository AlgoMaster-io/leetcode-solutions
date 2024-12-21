# [Leetcode 205: Isomorphic Strings](https://leetcode.com/problems/isomorphic-strings/)

## Approaches
1. [Basic Approach: Character Mapping Using Hashmaps](#basic-approach-character-mapping-using-hashmaps)
2. [Optimal Approach: Single Pass with Two Maps](#optimal-approach-single-pass-with-two-maps)

---

## Basic Approach: Character Mapping Using Hashmaps

### Intuition
The basic idea to solve the problem of determining whether two strings are isomorphic is to map every character of the first string `s` to a character in the second string `t`. For this, we can utilize hashmaps. We need two hashmaps: one to map characters from `s` to `t` and another to ensure that no duplicates exist in our mapping. This ensures that each character in `s` maps to a unique character in `t`.

### Steps
1. **Initialize two hashmaps**: `mapST` for mapping characters from `s` to `t`, and `mapTS` for mapping characters from `t` to `s`.
2. **Iterate over the strings**:
   - For each character pair `(s[i], t[i])`, check the following:
     - If `s[i]` is already mapped to a character in `t` and it doesn't match `t[i]`, the strings are not isomorphic.
     - If `t[i]` is already mapped to a character in `s` and it doesn't match `s[i]`, the strings are not isomorphic.
   - Otherwise, insert the mapping into both hashmaps.
3. **Return true if all character pairs are correctly mapped without conflicts**.

### Time Complexity
- **O(n)**: Where `n` is the length of the strings `s` and `t`, due to single pass linearly.

### Space Complexity
- **O(m + n)**: Where `m` and `n` are the number of unique characters in `s` and `t` to store mappings.

### Implementation
```cpp
#include <unordered_map>
#include <string>

bool isIsomorphic(std::string s, std::string t) {
    if (s.length() != t.length()) return false;

    std::unordered_map<char, char> mapST;
    std::unordered_map<char, char> mapTS;

    for (int i = 0; i < s.length(); ++i) {
        char sChar = s[i], tChar = t[i];
        
        // Check if there's already a mapping for sChar
        if (mapST.count(sChar) && mapST[sChar] != tChar) {
            return false;
        }
        
        // Check if there's already a mapping for tChar
        if (mapTS.count(tChar) && mapTS[tChar] != sChar) {
            return false;
        }

        // Create the mapping
        mapST[sChar] = tChar;
        mapTS[tChar] = sChar;
    }
    
    return true;
}
```

---

## Optimal Approach: Single Pass with Two Maps

### Intuition
The optimal solution leverages the same idea as the basic approach but can be seen as more streamlined. Instead of using two separate maps, we maintain a single pass while ensuring that both `s` to `t` and `t` to `s` mappings are checked simultaneously.

### Steps
1. **Initialize two arrays**: `mappedST` and `mappedTS` for mapping characters from `s` to `t` and vice versa, where each index corresponds to a character.
2. **Iterate over the strings**:
   - Directly access and update the mapping in constant time, ensuring both direct and reverse character mapping consistency.
   - If any conflict is found during mapping, return false.
3. **Return true if all mappings are consistent and no conflicts are detected**.

### Time Complexity
- **O(n)**: Single pass through the characters of the strings.

### Space Complexity
- **O(1)**: As the size of the array only depends on the character set (constant size for ASCII).

### Implementation
```cpp
#include <string>

bool isIsomorphic(std::string s, std::string t) {
    if (s.length() != t.length()) return false;

    char mappedST[256] = {0};
    char mappedTS[256] = {0};

    for (int i = 0; i < s.length(); ++i) {
        char sChar = s[i];
        char tChar = t[i];

        // Check two-way mappings
        if (mappedST[sChar] == 0 && mappedTS[tChar] == 0) {
            mappedST[sChar] = tChar;
            mappedTS[tChar] = sChar;
        } 
        else if (mappedST[sChar] != tChar || mappedTS[tChar] != sChar) {
            return false;
        }
    }

    return true;
}
```

Both approaches provide valid solutions to determine if two strings are isomorphic, but the optimal solution achieves this with constant space complexity due to character set limitations.

