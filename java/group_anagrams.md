# [49. Group Anagrams](https://leetcode.com/problems/group-anagrams/)

## Approach 1: Sorting + HashMap

### Solution
```java
// Time Complexity: O(n * k * log k) where n is the number of strings and k is the average string length
// Space Complexity: O(n * k)
import java.util.*;

public class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        // Map to group anagrams by their sorted representation
        HashMap<String, List<String>> map = new HashMap<>();

        for (String str : strs) {
            // Sort the string to create a key for anagrams
            char[] charArray = str.toCharArray();
            Arrays.sort(charArray);
            String sortedStr = new String(charArray);

            // Add the string to the corresponding group
            map.putIfAbsent(sortedStr, new ArrayList<>());
            map.get(sortedStr).add(str);
        }

        // Return all groups of anagrams
        return new ArrayList<>(map.values());
    }
}
```

## Approach 2: Frequency Count + HashMap

### Solution
```java
// Time Complexity: O(n * k) where n is the number of strings and k is the average string length
// Space Complexity: O(n * k)
import java.util.*;

public class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        // Map to group anagrams by their frequency count representation
        HashMap<String, List<String>> map = new HashMap<>();

        for (String str : strs) {
            // Create a frequency count array for the string
            int[] count = new int[26];
            for (char c : str.toCharArray()) {
                count[c - 'a']++;
            }

            // Convert frequency array to a string key
            StringBuilder keyBuilder = new StringBuilder();
            for (int freq : count) {
                keyBuilder.append(freq).append("#");
            }
            String key = keyBuilder.toString();

            // Add the string to the corresponding group
            map.putIfAbsent(key, new ArrayList<>());
            map.get(key).add(str);
        }

        // Return all groups of anagrams
        return new ArrayList<>(map.values());
    }
}
```

## Approach 3: Prime Product Hashing (Alternative)

### Solution
```java
// Time Complexity: O(n * k) where n is the number of strings and k is the average string length
// Space Complexity: O(n * k)
import java.util.*;

public class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        // Assign a unique prime number to each character
        int[] primes = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 
                        53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101};
        HashMap<Long, List<String>> map = new HashMap<>();

        for (String str : strs) {
            long product = 1;
            for (char c : str.toCharArray()) {
                product *= primes[c - 'a']; // Compute unique hash using prime products
            }

            // Add the string to the corresponding group
            map.putIfAbsent(product, new ArrayList<>());
            map.get(product).add(str);
        }

        // Return all groups of anagrams
        return new ArrayList<>(map.values());
    }
}
```