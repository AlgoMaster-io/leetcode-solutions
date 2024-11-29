# [49. Group Anagrams](https://leetcode.com/problems/group-anagrams/)

## Approach 1: Sorting + HashMap

### Solution
```cpp
// Time Complexity: O(n * k * log k) where n is the number of strings and k is the average string length
// Space Complexity: O(n * k)
#include <vector>
#include <string>
#include <unordered_map>
#include <algorithm>

using namespace std;

class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        // Map to group anagrams by their sorted representation
        unordered_map<string, vector<string>> map;

        for (string& str : strs) {
            // Sort the string to create a key for anagrams
            string sortedStr = str;
            sort(sortedStr.begin(), sortedStr.end());

            // Add the string to the corresponding group
            map[sortedStr].push_back(str);
        }

        // Return all groups of anagrams
        vector<vector<string>> result;
        for (auto& entry : map) {
            result.push_back(entry.second);
        }
        return result;
    }
};
```

## Approach 2: Frequency Count + HashMap

### Solution
```cpp
// Time Complexity: O(n * k) where n is the number of strings and k is the average string length
// Space Complexity: O(n * k)
#include <vector>
#include <string>
#include <unordered_map>

using namespace std;

class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        // Map to group anagrams by their frequency count representation
        unordered_map<string, vector<string>> map;

        for (string& str : strs) {
            // Create a frequency count array for the string
            vector<int> count(26, 0);
            for (char c : str) {
                count[c - 'a']++;
            }

            // Convert frequency array to a string key
            string key;
            for (int freq : count) {
                key += to_string(freq) + "#";
            }

            // Add the string to the corresponding group
            map[key].push_back(str);
        }

        // Return all groups of anagrams
        vector<vector<string>> result;
        for (auto& entry : map) {
            result.push_back(entry.second);
        }
        return result;
    }
};
```

## Approach 3: Prime Product Hashing (Alternative)

### Solution
```cpp
// Time Complexity: O(n * k) where n is the number of strings and k is the average string length
// Space Complexity: O(n * k)
#include <vector>
#include <string>
#include <unordered_map>

using namespace std;

class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        // Assign a unique prime number to each character
        int primes[] = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47,
                        53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101};
        unordered_map<long, vector<string>> map;

        for (string& str : strs) {
            long product = 1;
            for (char c : str) {
                product *= primes[c - 'a']; // Compute unique hash using prime products
            }

            // Add the string to the corresponding group
            map[product].push_back(str);
        }

        // Return all groups of anagrams
        vector<vector<string>> result;
        for (auto& entry : map) {
            result.push_back(entry.second);
        }
        return result;
    }
};
```

