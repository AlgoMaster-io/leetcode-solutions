# [Leetcode 1189: Maximum Number of Balloons](https://leetcode.com/problems/maximum-number-of-balloons/)

## Approaches
1. [Frequency Counter Approach](#frequency-counter-approach)

### Frequency Counter Approach

**Intuition:**

The problem involves determining how many times the word "balloon" can be formed using the characters from a given string. To achieve this, we should count the characters in both the given string and the target word "balloon" and then determine the number of complete sets of "balloon" we can form with the available characters.

Each occurrence of "balloon" requires:

- 1 'b'
- 1 'a'
- 2 'l's
- 2 'o's
- 1 'n'

Thus, if we can count the occurrences of these characters in the string, we can determine how many complete sets of "balloon" can be formed.

**Steps:**

1. Create a frequency counter for the characters in the string.
2. Define another counter for the characters needed from "balloon".
3. For each character needed, determine how many complete sets of "balloon" can be formed by dividing the character count from the frequency counter by the required count.
4. The minimum of these values across all required characters will tell us the maximum number of "balloon" instances we can form.

**Time Complexity:** O(n), where n is the length of the input string. We are iterating through the string and counting characters, which takes linear time.

**Space Complexity:** O(1), because the frequency maps have a fixed size related to the number of distinct characters of interest (at most 26 if considering the whole alphabet).

```typescript
function maxNumberOfBalloons(text: string): number {
    // Create a frequency map for required characters in "balloon"
    const balloonMap: Record<string, number> = {'b': 1, 'a': 1, 'l': 2, 'o': 2, 'n': 1};

    // Initialize a frequency map for counting characters in the input text
    const textMap: Record<string, number> = {};

    // Count each character in the input text
    for (let char of text) {
        if (textMap[char] === undefined) {
            textMap[char] = 0;
        }
        textMap[char]++;
    }

    // Determine the maximum number of "balloon" words that can be formed
    let maxBalloons = Infinity;

    // For each required character, calculate how many times it can contribute to forming "balloon"
    for (let char in balloonMap) {
        if (textMap[char] === undefined) {
            maxBalloons = 0; // If any character is missing, no "balloon" can be formed
            break;
        }
        // Integer division to find full instances of character contribution
        const possibleBalloons = Math.floor(textMap[char] / balloonMap[char]);
        maxBalloons = Math.min(maxBalloons, possibleBalloons);
    }

    return maxBalloons;
}
```

This approach efficiently checks how many times we can form the required number of characters to spell the word "balloon" from the input string while utilizing a character frequency map to minimize repetitive operations.

