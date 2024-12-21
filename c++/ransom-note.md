# [Leetcode 383: Ransom Note](https://leetcode.com/problems/ransom-note/)

## Table of Contents
1. [Approach 1: Frequency Count Using Arrays](#approach-1)
2. [Approach 2: Frequency Count Using Hash Map (unordered_map)](#approach-2)

---

## Approach 1: Frequency Count Using Arrays

### Intuition
The problem can be broken down by comparing the frequency of each character in both the `ransomNote` and `magazine`. If for any character the `ransomNote` requires a count that is greater than what `magazine` can provide, it's impossible to construct the ransom note.

### Steps
1. Create a frequency array for `magazine` to count each character's occurrence.
2. Iterate through `ransomNote` and for each character, check if it can be provided by `magazine`.
3. If yes, decrement the corresponding count, otherwise return `false`.

### Code
```cpp
#include <vector>
#include <string>

bool canConstruct(std::string ransomNote, std::string magazine) {
    // Frequency array to store number of each character present 'a' to 'z'
    std::vector<int> frequency(26, 0);

    // Fill the frequency array with character counts from magazine
    for (char c : magazine) {
        frequency[c - 'a']++;
    }

    // Check each character in ransomNote
    for (char c : ransomNote) {
        // If the character is not available in magazine's frequency array, return false
        if (frequency[c - 'a'] <= 0) {
            return false;
        }
        // Decrement the count in frequency array since one instance is used
        frequency[c - 'a']--;
    }

    // If we can go through the ransomNote, it is constructable
    return true;
}
```

### Complexity Analysis
- **Time Complexity**: O(n + m), where `n` is the length of `ransomNote` and `m` is the length of `magazine`.
- **Space Complexity**: O(1), since the size of `frequency` is constant (26 letters).

---

## Approach 2: Frequency Count Using Hash Map (unordered_map)

### Intuition
A similar method to Approach 1, but using a hash map (`unordered_map`) to store character frequencies. This approach is more flexible for scenarios involving potential non-English characters but comes at the cost of additional overhead.

### Steps
1. Use an unordered map to store the frequency of characters from `magazine`.
2. Traverse the `ransomNote` to check if each character's demand can be met by `magazine`.
3. Return false if at any time a character is insufficient or missing.

### Code
```cpp
#include <unordered_map>
#include <string>

bool canConstruct(std::string ransomNote, std::string magazine) {
    // A hashmap to store frequency of each character in magazine
    std::unordered_map<char, int> frequency;

    // Counting occurrences of each character in magazine
    for (char c : magazine) {
        frequency[c]++;
    }

    // Iterating through ransomNote to see if each character can be met
    for (char c : ransomNote) {
        // If the character is not present or exhausted
        if (frequency[c] <= 0) {
            return false;
        }
        // Use one of the characters
        frequency[c]--;
    }

    return true;
}
```

### Complexity Analysis
- **Time Complexity**: O(n + m), where `n` is the length of `ransomNote` and `m` is the length of `magazine`.
- **Space Complexity**: O(1), since there are a finite number of characters and the map does not grow with input size. The effective space usage might be slightly higher due to the unordered_map overhead.


