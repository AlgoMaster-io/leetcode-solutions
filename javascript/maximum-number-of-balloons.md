## Problem: [1189. Maximum Number of Balloons](https://leetcode.com/problems/maximum-number-of-balloons/)

## Approaches
- [Solution 1: Frequency Counting](#solution-1-frequency-counting)
- [Solution 2: Optimized Single Pass](#solution-2-optimized-single-pass)

### Solution 1: Frequency Counting

#### Intuition:
The problem requires us to determine how many instances of the word "balloon" can be formed from the characters of a given string. This can be achieved simply by counting the frequency of each character required to form "balloon" and checking how many complete words can be formed using these frequencies.

The word "balloon" requires:
- 1 'b'
- 1 'a'
- 2 'l's
- 2 'o's
- 1 'n'

We'll count the occurrences of these specific characters in the input string and then calculate the minimum number of times we can form "balloon".

#### Code:
```javascript
function maxNumberOfBalloons(text) {
    // Initialize a frequency dictionary for the characters needed in "balloon"
    const charCount = { 'b': 0, 'a': 0, 'l': 0, 'o': 0, 'n': 0 };

    // Count each character in the provided text
    for (let char of text) {
        if (char in charCount) {
            charCount[char] += 1;
        }
    }

    // Calculate how many complete 'balloon' words can be formed
    let maxBalloons = Infinity;
    maxBalloons = Math.min(maxBalloons, charCount['b']);
    maxBalloons = Math.min(maxBalloons, charCount['a']);
    maxBalloons = Math.min(maxBalloons, Math.floor(charCount['l'] / 2));
    maxBalloons = Math.min(maxBalloons, Math.floor(charCount['o'] / 2));
    maxBalloons = Math.min(maxBalloons, charCount['n']);

    return maxBalloons;
}
```

#### Time and Space Complexity:
- **Time Complexity**: O(n), where n is the length of the input string, because we iterate through the string once.
- **Space Complexity**: O(1), since we are using a fixed size dictionary to store character counts.

### Solution 2: Optimized Single Pass

#### Intuition:
This approach is an optimization over the previous solution by reducing unnecessary checks. During the string traversal, instead of revisiting the charCount for calculating the minimum, we can keep track of the potential "balloon" formations as we update the counts.

#### Code:
```javascript
function maxNumberOfBalloons(text) {
    // Initialize a frequency dictionary to keep track of counts during traversal
    let b = 0, a = 0, l = 0, o = 0, n = 0;

    // Traverse the input text to update frequencies
    for (let char of text) {
        switch (char) {
            case 'b': b++; break;
            case 'a': a++; break;
            case 'l': l++; break;
            case 'o': o++; break;
            case 'n': n++; break;
        }
    }

    // Calculate how many complete 'balloon's can be formed
    // 'l' and 'o' are divided by 2 because we need 2 of each for one 'balloon'
    return Math.min(b, a, Math.floor(l / 2), Math.floor(o / 2), n);
}
```

#### Time and Space Complexity:
- **Time Complexity**: O(n), where n is the length of the input string, as we iterate through the string once.
- **Space Complexity**: O(1), since no additional space is needed for processing, and we only use a few variables to track counts.

With these solutions, we effectively tackle the problem of finding the maximum number of the word "balloon" that can be formed from a given string `text` by leveraging efficient counting techniques.

