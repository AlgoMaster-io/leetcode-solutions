# [Leetcode 383: Ransom Note](https://leetcode.com/problems/ransom-note/)

## Approaches
- [Approach 1: Count Characters using Dictionary](#approach-1-count-characters-using-dictionary)
- [Approach 2: Count Characters using Array](#approach-2-count-characters-using-array)

## Approach 1: Count Characters using Dictionary

### Intuition
The basic idea is to use a dictionary to keep track of the count of each character in the `magazine`. Then, for each character in the `ransomNote`, check if the character exists in the dictionary with a non-zero value. If it does, decrement the count in the dictionary. If at any point a required character is not found or has zero count, return false.

### Solution in C#

```csharp
public class Solution {
    public bool CanConstruct(string ransomNote, string magazine) {
        // Dictionary to track the frequency of each character in the magazine
        Dictionary<char, int> charCount = new Dictionary<char, int>();
        
        // Count each character in the magazine
        foreach (char c in magazine) {
            if (charCount.ContainsKey(c)) {
                charCount[c]++;
            } else {
                charCount[c] = 1;
            }
        }

        // Check if we can construct ransomNote using the characters from the magazine
        foreach (char c in ransomNote) {
            // If the character is not available in magazine or exhausted, return false
            if (!charCount.ContainsKey(c) || charCount[c] == 0) {
                return false;
            }
            // Decrement character count from magazine
            charCount[c]--;
        }

        // All characters from ransomNote are available
        return true;
    }
}
```

### Time Complexity
- The time complexity is O(m + n), where `m` is the length of the `magazine` and `n` is the length of the `ransomNote`.

### Space Complexity
- The space complexity is O(k), where `k` is the number of unique characters in the `magazine`.

## Approach 2: Count Characters using Array

### Intuition
As `ransomNote` and `magazine` contain only lowercase letters, we can optimize our solution further using an array instead of a dictionary. Each index of the array corresponds to a character ('a' to 'z'), allowing direct indexing and decreasing overhead of dictionary operations.

### Solution in C#

```csharp
public class Solution {
    public bool CanConstruct(string ransomNote, string magazine) {
        // Array to track the frequency of each character in the magazine
        int[] charCount = new int[26];

        // Count each character in the magazine
        foreach (char c in magazine) {
            // Increment the count at the index corresponding to the character
            charCount[c - 'a']++;
        }

        // Check if we can construct ransomNote using the characters from the magazine
        foreach (char c in ransomNote) {
            // If the character is not available in magazine or exhausted, return false
            if (charCount[c - 'a'] <= 0) {
                return false;
            }
            // Decrement character count from magazine
            charCount[c - 'a']--;
        }

        // All characters from ransomNote are available
        return true;
    }
}
```

### Time Complexity
- The time complexity is O(m + n), where `m` is the length of the `magazine` and `n` is the length of the `ransomNote`.

### Space Complexity
- The space complexity is O(1) because the size of the array is fixed to 26, corresponding to the 26 lowercase English letters.

