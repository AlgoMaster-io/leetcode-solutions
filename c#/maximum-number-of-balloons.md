# Leetcode Problem: [Maximum Number of Balloons](https://leetcode.com/problems/maximum-number-of-balloons/)

## Approaches

1. [Basic Count and Subtract Method](#basic-count-and-subtract-method)
2. [Frequency Dictionary Method](#frequency-dictionary-method)

---

### Basic Count and Subtract Method

**Intuition**

To determine how many times the word "balloon" can be formed, we need to count each letter in the given string and then check the minimum number of times we can assemble "balloon" using these counts. The simplest approach is to directly count the required letters 'b', 'a', 'l', 'o', 'n' from the input string, and then calculate how many times "balloon" can be made with that count.

**Steps**
1. Count the occurrences of each character that appears in "balloon".
2. Determine how many 'b's, 'a's, 'l's, 'o's, and 'n's are required to form "balloon".
3. Calculate the minimum full "balloon" words that can be assembled by dividing the character counts (we divide the count of 'l' and 'o' by 2 since they appear twice in "balloon").

**Code**

```csharp
public class Solution {
    public int MaxNumberOfBalloons(string text) {
        // Initialize counters for each letter in "balloon"
        int b = 0, a = 0, l = 0, o = 0, n = 0;
        
        // Count occurrences of each character in the string
        foreach (char c in text) {
            if (c == 'b') b++;
            if (c == 'a') a++;
            if (c == 'l') l++;
            if (c == 'o') o++;
            if (c == 'n') n++;
        }

        // Calculate minimum number of "balloon"
        return Math.Min(b, Math.Min(a, Math.Min(l / 2, Math.Min(o / 2, n))));
    }
}
```

**Time Complexity**: O(n), where n is the length of the input string `text` since we traverse the string once.

**Space Complexity**: O(1), since we are using a fixed amount of extra space (for the letter counters).

---

### Frequency Dictionary Method

**Intuition**

This method also involves counting the occurrences of each character necessary for "balloon", but we utilize a dictionary to track frequency counts. This can sometimes be more flexible or readable, especially if the word changes or expands to include more characters.

**Steps**
1. Create a frequency dictionary for the required characters.
2. Count the occurrences of each character in the dictionary.
3. Calculate the possible number of "balloon" using the frequency dictionary by considering the constraints that 'l' and 'o' need to appear twice.

**Code**

```csharp
public class Solution {
    public int MaxNumberOfBalloons(string text) {
        // Dictionary to store the count of each letter required to form "balloon"
        var freq = new Dictionary<char, int>();
        freq.Add('b', 0);
        freq.Add('a', 0);
        freq.Add('l', 0);
        freq.Add('o', 0);
        freq.Add('n', 0);
        
        // Count occurrences of each character
        foreach (char c in text) {
            if (freq.ContainsKey(c)) {
                freq[c]++;
            }
        }

        // Calculate minimum number of "balloon" that can be formed
        int minBalloons = int.MaxValue;
        minBalloons = Math.Min(minBalloons, freq['b']);
        minBalloons = Math.Min(minBalloons, freq['a']);
        minBalloons = Math.Min(minBalloons, freq['l'] / 2);
        minBalloons = Math.Min(minBalloons, freq['o'] / 2);
        minBalloons = Math.Min(minBalloons, freq['n']);
        
        return minBalloons;
    }
}
```

**Time Complexity**: O(n), where n is the length of the input string `text` since we traverse the string once.

**Space Complexity**: O(1), because the dictionary maintains a fixed number of entries, independent of `n`.

