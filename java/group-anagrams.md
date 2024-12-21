# [Leetcode 49: Group Anagrams](https://leetcode.com/problems/group-anagrams/)

## Solutions:
- [Approach 1: Sorting Each Word](#approach-1-sorting-each-word)
- [Approach 2: Counting Characters](#approach-2-counting-characters)

### Approach 1: Sorting Each Word

**Intuition:**

The first idea is to use the property that if two strings are anagrams of each other, their sorted form will be the same. By sorting each string and using it as a key, we can group the words that are anagrams together.

1. Iterate over the list of strings.
2. Sort each string and use it as a key in a hashmap where the value will be a list of strings.
3. Finally, return all the values of the hashmap that represent grouped anagrams.

**Code:**

```java
import java.util.*;

class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        // Map to store the list of anagrams
        Map<String, List<String>> anagramMap = new HashMap<>();
        
        for (String word : strs) {
            // Convert the word to an array of characters
            char[] charArray = word.toCharArray();
            // Sort the array
            Arrays.sort(charArray);
            // Convert back to string
            String sortedWord = new String(charArray);
            
            // If the sorted word is not in the map, add it with an empty list
            if (!anagramMap.containsKey(sortedWord)) {
                anagramMap.put(sortedWord, new ArrayList<>());
            }
            
            // Append the original word to the corresponding list
            anagramMap.get(sortedWord).add(word);
        }
        
        // Return the grouped list of anagrams
        return new ArrayList<>(anagramMap.values());
    }
}
```

**Time Complexity:** O(NK log K) where N is the number of strings and K is the maximum length of a string. Sorting each string takes O(K log K).

**Space Complexity:** O(NK), the space for storing the groups of anagrams.

### Approach 2: Counting Characters

**Intuition:**

Instead of sorting, we can use the frequency of characters as a key. If two strings are anagrams, they will have the same frequency distribution of characters.

1. Use an array of size 26 to count the frequency of each character for each string because all characters are lowercase.
2. Convert this frequency array to a unique string and use it as a key in a hashmap.
3. Group strings with the same frequency distribution together.

**Code:**

```java
import java.util.*;

class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        // Main map to track groups of anagrams
        Map<String, List<String>> anagramMap = new HashMap<>();
        
        for (String word : strs) {
            // 26 letters in the alphabet
            int[] charCount = new int[26];
            
            // Count each character's frequency in the word
            for (char c : word.toCharArray()) {
                charCount[c - 'a']++;
            }
            
            // Build key based on character counts
            StringBuilder keyBuilder = new StringBuilder();
            for (int count : charCount) {
                keyBuilder.append('#');
                keyBuilder.append(count);
            }
            String key = keyBuilder.toString();
            
            // Add the word to the relevant anagram group
            if (!anagramMap.containsKey(key)) {
                anagramMap.put(key, new ArrayList<>());
            }
            anagramMap.get(key).add(word);
        }
        
        // Return all grouped anagrams
        return new ArrayList<>(anagramMap.values());
    }
}
```

**Time Complexity:** O(NK), as constructing the unique string key per word from its character count takes O(K).

**Space Complexity:** O(NK), storing the groups of anagrams based on their character frequency in the hashmap.

