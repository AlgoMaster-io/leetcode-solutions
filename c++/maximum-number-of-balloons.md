# [Leetcode 1189: Maximum Number of Balloons](https://leetcode.com/problems/maximum-number-of-balloons/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Optimized Frequency Count](#approach-2-optimized-frequency-count)

### Approach 1: Brute Force

**Intuition**

The problem involves forming the word "balloon" from the characters provided in a string. To solve it using a brute force approach, we can count the frequency of each character in the given string and determine how many times "balloon" can be formed.

**Pseudocode**

1. Count the frequency of each character in the string.
2. Find the minimum number of times we can use these characters to form the word "balloon" by dividing the frequency needed in "balloon" by the frequency available in the string for each character.

**Code**

```cpp
class Solution {
public:
    int maxNumberOfBalloons(string text) {
        // Frequency array for the input text where index 0 to 25 represent 'a' to 'z'
        vector<int> freq(26, 0);

        // Count frequency of each character in the input text
        for(char ch : text) {
            freq[ch - 'a']++;
        }

        // Calculate the minimum number of "balloon" possible
        // 'b', 'a', 'n' each occurs once in "balloon", so need their counts directly.
        // 'l' and 'o' each occur twice in "balloon", so need half their counts.
        int b = freq['b' - 'a'];
        int a = freq['a' - 'a'];
        int l = freq['l' - 'a'] / 2;
        int o = freq['o' - 'a'] / 2;
        int n = freq['n' - 'a'];

        // Return the minimum of all these counts
        return min({b, a, l, o, n});
    }
};
```

**Time Complexity:** O(N), where N is the length of the string `text` (since we only need to traverse it a couple of times).

**Space Complexity:** O(1), because we only use a fixed amount of extra space (the frequency array).

### Approach 2: Optimized Frequency Count

**Intuition**

The brute force approach itself uses efficient counting, being linear, but handling the counts in a more organized manner could improve readability and maintainability. This approach initializes a count map for the necessary characters and uses that directly for calculation.

**Pseudocode**

1. Initialize a map to count how each necessary character is required from the word "balloon".
2. Count the characters in the given string.
3. Calculate the maximum number of times "balloon" can be formed by finding the minimum divide of character frequencies in the string and the word.

**Code**

```cpp
class Solution {
public:
    int maxNumberOfBalloons(string text) {
        unordered_map<char, int> count = {{'b', 0}, {'a', 0}, {'l', 0}, {'o', 0}, {'n', 0}};

        for (char ch : text) {
            if (count.find(ch) != count.end()) { // Only increment if the character is part of "balloon"
                count[ch]++;
            }
        }

        // "balloon" requires 'l' and 'o' twice
        count['l'] /= 2;
        count['o'] /= 2;

        // Return the minimum among these calculated counts
        int maxBalloons = INT_MAX;
        for (auto& p : count) {
            maxBalloons = min(maxBalloons, p.second);
        }

        return maxBalloons;
    }
};
```

**Time Complexity:** O(N), where N is the length of the string `text`.

**Space Complexity:** O(1), as the space used for counting is constant and does not grow with input size.

Both approaches offer linear time complexity, which is optimal for this problem given the constraint of counting characters. The primary optimization in the second approach is in organization and readability for handling frequency calculations.

