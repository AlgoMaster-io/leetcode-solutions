# [Leetcode Problem 1189: Maximum Number of Balloons](https://leetcode.com/problems/maximum-number-of-balloons/)

## Approaches:
- [Approach 1: Frequency Count with Maps](#approach-1-frequency-count-with-maps)
- [Approach 2: Optimized Frequency Count with Arrays](#approach-2-optimized-frequency-count-with-arrays)

### Approach 1: Frequency Count with Maps

**Intuition:**

- The word "balloon" consists of the letters: 'b', 'a', 'l', 'o', 'n'.
- We need to count these letters in the input string and determine how many times we can form the word "balloon".
- For 'l' and 'o', we need the count divided by 2 because these letters appear twice in "balloon".

**Approach:**

1. Create a frequency map for the characters in the input string.
2. Check the count of each character needed for "balloon".
3. The count of possible "balloons" that can be formed is determined by the minimum ratio of available to needed instances of these characters.

**Code:**

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public int maxNumberOfBalloons(String text) {
        // Create a frequency map for the input string
        Map<Character, Integer> freqMap = new HashMap<>();
        for (char c : text.toCharArray()) {
            freqMap.put(c, freqMap.getOrDefault(c, 0) + 1);
        }
        
        // Store the required frequencies of each character in 'balloon'
        String balloon = "balloon";
        
        // Calculate the max number of "balloon" words
        int maxBalloons = Integer.MAX_VALUE;
        Map<Character, Integer> balloonFreq = new HashMap<>();
        for (char c : balloon.toCharArray()) {
            balloonFreq.put(c, balloonFreq.getOrDefault(c, 0) + 1);
        }
        
        // Calculate the max possible number of "balloon" we can form
        for (Map.Entry<Character, Integer> e : balloonFreq.entrySet()) {
            char key = e.getKey();
            int count = e.getValue();
            maxBalloons = Math.min(maxBalloons, freqMap.getOrDefault(key, 0) / count);
        }
        
        return maxBalloons;
    }
}
```

**Complexity Analysis:**

- **Time Complexity:** O(n), where n is the length of the input string; we traverse the string to build the frequency map.
- **Space Complexity:** O(1), since the frequency map only contains a fixed number of characters (at most the size of the alphabet).

### Approach 2: Optimized Frequency Count with Arrays

**Intuition:**

- Similar to Approach 1, but instead of using a map, we can utilize an array to reduce space and improve time efficiency.
- Map the necessary characters directly to specific indices.

**Approach:**

1. Use an array of size 26 to count character frequencies (assuming only lowercase letters).
2. Use specific indices for the characters in "balloon": 'a', 'b', 'l', 'o', 'n'.
3. Calculate the minimum number of complete "balloon" strings using simple arithmetic operations on the counts of these characters.

**Code:**

```java
public class Solution {
    public int maxNumberOfBalloons(String text) {
        // Frequency array for all lowercase characters
        int[] count = new int[26];
        for (char c : text.toCharArray()) {
            count[c - 'a']++;
        }
        
        // Calculate the minimum number of "balloon" we can form
        int minBalloons = Integer.MAX_VALUE;
        
        // Check against required characters
        minBalloons = Math.min(minBalloons, count['b' - 'a']);
        minBalloons = Math.min(minBalloons, count['a' - 'a']);
        minBalloons = Math.min(minBalloons, count['l' - 'a'] / 2);
        minBalloons = Math.min(minBalloons, count['o' - 'a'] / 2);
        minBalloons = Math.min(minBalloons, count['n' - 'a']);
        
        return minBalloons;
    }
}
```

**Complexity Analysis:**

- **Time Complexity:** O(n), where n is the length of the input string; similar to approach 1 but simplified.
- **Space Complexity:** O(1), as we use a fixed-size array independent of the input size.

Both approaches aim to calculate the maximum number of times the string "balloon" can be formed from the input characters, focusing on counting the necessary characters efficiently. Approach 2 is more space-efficient and slightly more optimized in practice.

