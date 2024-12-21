
[Leetcode Problem 438: Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

## Approaches
- [Sliding Window with Frequency Count](#sliding-window-with-frequency-count)

### Sliding Window with Frequency Count

#### Intuition:
The problem is essentially about detecting when two substrings are anagrams. We can use the sliding window technique because the problem involves finding contiguous subarrays (anagrams). By maintaining a frequency count of the characters in the current window and the target string, 'p', we should be able to quickly check if the window is an anagram.

1. **Initial Setup**:
   - Calculate the frequency of each character in the string `p` because we need the anagram's frequency.
   - Use two arrays to track character frequencies:
     - `pCount` for the frequency of characters in `p`.
     - `sCount` for the frequency of characters in the current window of `s`.

2. **Sliding Window Technique**:
   - Use a fixed-size sliding window that spans the length of `p`.
   - Start by filling the window and calculate the frequencies in `sCount`.
   - As you slide the window one character at a time:
     - Compare the `sCount` and `pCount` to check if the current window's character frequency matches with `p`'s character frequency.
     - If they match, append the starting index of the window to the result list.

3. **Optimization**:
   - Instead of checking the entire `sCount` and `pCount` each time, keep an integer (`matches`) to check how many characters' frequencies match between `sCount` and `pCount`. Adjust `matches` as characters enter and leave the window.

Here is the implementation:

```javascript
function findAnagrams(s, p) {
    const result = [];
    if (s.length < p.length) return result;

    const pCount = new Array(26).fill(0);
    const sCount = new Array(26).fill(0);

    // Calculate frequency of each character in p
    for (let char of p) {
        pCount[char.charCodeAt(0) - 'a'.charCodeAt(0)]++;
    }

    let matches = 0;

    // Build the initial window in sCount with the first p.length characters of s
    for (let i = 0; i < p.length; i++) {
        let index = s[i].charCodeAt(0) - 'a'.charCodeAt(0);
        sCount[index]++;
    }

    // Check if initial window matches
    for (let i = 0; i < 26; i++) {
        if (pCount[i] === sCount[i]) matches++;
    }

    // Start sliding the window
    for (let i = 0; i < s.length - p.length; i++) {
        if (matches === 26) result.push(i);

        let lIndex = s[i].charCodeAt(0) - 'a'.charCodeAt(0);
        let rIndex = s[i + p.length].charCodeAt(0) - 'a'.charCodeAt(0);

        // Slide the window right: Remove leftmost character, add rightmost new character
        sCount[rIndex]++;
        if (sCount[rIndex] === pCount[rIndex]) {
            matches++;
        } else if (sCount[rIndex] === pCount[rIndex] + 1) {
            matches--;
        }

        sCount[lIndex]--;
        if (sCount[lIndex] === pCount[lIndex]) {
            matches++;
        } else if (sCount[lIndex] === pCount[lIndex] - 1) {
            matches--;
        }
    }

    // Check last window
    if (matches === 26) result.push(s.length - p.length);

    return result;
}
```

**Time Complexity**: O(N + M), where N is the length of `s` and M is the length of `p`. We pass through `s` a few times, but mostly calculate values in constant time for the 26 alphabet characters.

**Space Complexity**: O(1) because we use fixed-size arrays of size 26 for both `sCount` and `pCount`, which is independent of input size.

