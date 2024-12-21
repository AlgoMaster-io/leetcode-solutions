# [Leetcode 49: Group Anagrams](https://leetcode.com/problems/group-anagrams/)

## Approaches
- [Approach 1: Sorting Each Word](#approach-1-sorting-each-word)
- [Approach 2: Character Count Signature](#approach-2-character-count-signature)

## Approach 1: Sorting Each Word

### Intuition
The anagrams of a word are just permutations of the letters. Thus, when sorted, all anagrams will become identical. We can use this property to group anagrams by sorting each word and using the sorted word as a key in a dictionary. 

### Solution
We will iterate over the input list of strings, sort each string and use it as a key in a dictionary to group words having the same sorted characters. The values of the dictionary will be lists of anagrams. Finally, we will return the list of values from the dictionary.

### Code
```csharp
using System;
using System.Collections.Generic;
using System.Linq;

public class Solution {
    public IList<IList<string>> GroupAnagrams(string[] strs) {
        // Dictionary to store anagrams groups
        Dictionary<string, List<string>> map = new Dictionary<string, List<string>>();
        
        foreach (string s in strs) {
            // Sort each word
            char[] arr = s.ToCharArray();
            Array.Sort(arr);
            string sorted = new string(arr);
            
            // If the sorted string is not in the map, add it
            if (!map.ContainsKey(sorted)) {
                map[sorted] = new List<string>();
            }
            
            // Add the original string to the correct anagram group
            map[sorted].Add(s);
        }
        
        // Convert the map values to a list of lists
        return new List<IList<string>>(map.Values);
    }
}
```

### Complexity
- **Time Complexity**: \(O(NK \log K)\)
  - Sorting each word takes \(O(K \log K)\), where \(K\) is the maximum length of a word.
  - We do this for all \(N\) words.
- **Space Complexity**: \(O(NK)\)
  - The space required to store the result and the dictionary.

## Approach 2: Character Count Signature

### Intuition
Instead of sorting, we can use a frequency count of characters as a signature for anagrams. Two strings are anagrams if and only if their character counts are the same.

### Solution
We create a list of size 26 (for each letter in the alphabet) to store the character count. For each string, we compute the character count and use it as a key in a dictionary to store all anagrams sharing the same character count.

### Code
```csharp
using System;
using System.Collections.Generic;

public class Solution {
    public IList<IList<string>> GroupAnagrams(string[] strs) {
        // Dictionary to store anagrams groups by character count signature
        Dictionary<string, List<string>> map = new Dictionary<string, List<string>>();
        
        foreach (string s in strs) {
            // Initialize the character count
            int[] count = new int[26];
            foreach (char c in s) {
                count[c - 'a']++;
            }
            
            // Create a signature string from the character count
            string key = string.Join(",", count);
            
            // If the signature is not in the map, add it
            if (!map.ContainsKey(key)) {
                map[key] = new List<string>();
            }
            
            // Add the original string to the correct anagram group
            map[key].Add(s);
        }
        
        // Convert the map values to a list of lists
        return new List<IList<string>>(map.Values);
    }
}
```

### Complexity
- **Time Complexity**: \(O(NK)\)
  - Constructing the character count signature takes \(O(K)\) for each word.
  - We do this for all \(N\) words.
- **Space Complexity**: \(O(NK)\)
  - The space required to store the result and the dictionary.

This approach is often faster than sorting-based methods because it avoids the \( \log K \) factor. However, it requires careful handling of the character count signature to avoid collisions.

