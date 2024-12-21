## [Leetcode Problem 49: Group Anagrams](https://leetcode.com/problems/group-anagrams/)

### Approaches:
- [Approach 1: Sorting](#approach-1-sorting)
- [Approach 2: Character Count](#approach-2-character-count)

### Approach 1: Sorting
#### Intuition:
To determine if two strings are anagrams, one simple method is to sort both strings and see if they become identical. Therefore, we can sort each string in the list and use the sorted string as a key in a hashmap. Strings that are anagrams of each other will result in the same sorted string and thus belong to the same key.

#### Steps:
1. Initialize an empty hashmap where each key is a sorted string and its value is a list of anagrams.
2. Iterate over each string, sort it and use it as a key.
3. Append the original string to the value list under this key.
4. Return the values of the hashmap, which are lists of anagrams.

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <unordered_map>
#include <algorithm>

std::vector<std::vector<std::string>> groupAnagrams(std::vector<std::string>& strs) {
    std::unordered_map<std::string, std::vector<std::string>> anagrams;
    
    for (const std::string& s : strs) {
        // Sort the string
        std::string sorted_str = s;
        std::sort(sorted_str.begin(), sorted_str.end());

        // Use sorted string as key, and store original string in respective list
        anagrams[sorted_str].emplace_back(s);
    }
    
    // Prepare result from hashmap values
    std::vector<std::vector<std::string>> result;
    for (const auto& pair : anagrams) {
        result.emplace_back(pair.second);
    }
    
    return result;
}
```
- **Time Complexity:** \(O(n \cdot k \log k)\), where \(n\) is the number of strings and \(k\) is the maximum length of a string. We sort each string, consuming \(O(k \log k)\).
- **Space Complexity:** \(O(n \cdot k)\) to store the strings in the hashmap.

### Approach 2: Character Count
#### Intuition:
Instead of sorting the strings, another approach is to use a fixed size array (or hashmap) to count the occurrences of each character. This count serves as a unique identifier for the anagram group.

#### Steps:
1. Initialize an empty hashmap with keys being the character counts and values being the anagram groups.
2. For each string, use an integer array of size 26 (for each letter in the alphabet) to record the frequency of each letter.
3. Use the character count array itself (or a possible hash of it) as a key in the hashmap.
4. Append the original string to the value list under this key.
5. Return the values of the hashmap.

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <unordered_map>

std::vector<std::vector<std::string>> groupAnagrams(std::vector<std::string>& strs) {
    std::unordered_map<std::string, std::vector<std::string>> anagrams;

    for (const std::string& s : strs) {
        // Create a character count vector
        std::vector<int> counts(26, 0);
        
        // Count each character in the string
        for (const char& c : s) {
            counts[c - 'a']++;
        }
        
        // Convert character count array to a string to use as hashmap key
        std::string key = "";
        for (int count : counts) {
            key += '#' + std::to_string(count); // form a key with delimiters
        }
        
        anagrams[key].emplace_back(s);
    }
    
    // Prepare result from hashmap values
    std::vector<std::vector<std::string>> result;
    for (const auto& pair : anagrams) {
        result.emplace_back(pair.second);
    }
    
    return result;
}
```
- **Time Complexity:** \(O(n \cdot k)\), where \(n\) is the number of strings and \(k\) is the maximum length of a string. Counting each string consumes \(O(k)\).
- **Space Complexity:** \(O(n \cdot k)\), for storing the strings and the hashmap keys.

Both approaches provide robust solutions to group anagrams, with the second one being more efficient when dealing with longer strings due to the character count approach avoiding sorting.

