# [49. Group Anagrams](https://leetcode.com/problems/group-anagrams/)

## Approach 1: Sorting + HashMap

### Solution
csharp
```csharp
// Time Complexity: O(n * k * log k) where n is the number of strings and k is the average string length
// Space Complexity: O(n * k)
using System;
using System.Collections.Generic;

public class Solution {
    public IList<IList<string>> GroupAnagrams(string[] strs) {
        // Dictionary to group anagrams by their sorted representation
        var map = new Dictionary<string, List<string>>();

        foreach (string str in strs) {
            // Sort the string to create a key for anagrams
            char[] charArray = str.ToCharArray();
            Array.Sort(charArray);
            string sortedStr = new string(charArray);

            // Add the string to the corresponding group
            if (!map.ContainsKey(sortedStr)) {
                map[sortedStr] = new List<string>();
            }
            map[sortedStr].Add(str);
        }

        // Return all groups of anagrams
        return new List<IList<string>>(map.Values);
    }
}
```

## Approach 2: Frequency Count + HashMap

### Solution
csharp
```csharp
// Time Complexity: O(n * k) where n is the number of strings and k is the average string length
// Space Complexity: O(n * k)
using System;
using System.Collections.Generic;
using System.Text;

public class Solution {
    public IList<IList<string>> GroupAnagrams(string[] strs) {
        // Dictionary to group anagrams by their frequency count representation
        var map = new Dictionary<string, List<string>>();

        foreach (string str in strs) {
            // Create a frequency count array for the string
            int[] count = new int[26];
            foreach (char c in str) {
                count[c - 'a']++;
            }

            // Convert frequency array to a string key
            StringBuilder keyBuilder = new StringBuilder();
            foreach (int freq in count) {
                keyBuilder.Append(freq).Append('#');
            }
            string key = keyBuilder.ToString();

            // Add the string to the corresponding group
            if (!map.ContainsKey(key)) {
                map[key] = new List<string>();
            }
            map[key].Add(str);
        }

        // Return all groups of anagrams
        return new List<IList<string>>(map.Values);
    }
}
```

## Approach 3: Prime Product Hashing (Alternative)

### Solution
csharp
```csharp
// Time Complexity: O(n * k) where n is the number of strings and k is the average string length
// Space Complexity: O(n * k)
using System;
using System.Collections.Generic;

public class Solution {
    public IList<IList<string>> GroupAnagrams(string[] strs) {
        // Assign a unique prime number to each character
        int[] primes = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 
                        53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101};
        var map = new Dictionary<long, List<string>>();

        foreach (string str in strs) {
            long product = 1;
            foreach (char c in str) {
                product *= primes[c - 'a']; // Compute unique hash using prime products
            }

            // Add the string to the corresponding group
            if (!map.ContainsKey(product)) {
                map[product] = new List<string>();
            }
            map[product].Add(str);
        }

        // Return all groups of anagrams
        return new List<IList<string>>(map.Values);
    }
}
```

