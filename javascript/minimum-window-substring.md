# [Leetcode 76: Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

## Approaches:
- [Brute Force Approach](#brute-force-approach)
- [Sliding Window Optimal Approach](#sliding-window-optimal-approach)

---

### Brute Force Approach

**Intuition**:  
The brute force approach involves generating all possible substrings of the given string `s` and then checking each one to see if it contains all characters of `t`. If it does, compare its length to the shortest valid substring found so far, and update if it's shorter.

**Steps**:
1. Iterate through all possible starting points of substrings in `s`.
2. For each starting point, iterate through all possible ending points, generating substrings.
3. For each generated substring, verify if it contains all characters in `t` using a frequency count.
4. Maintain and update a record of the minimum length valid substring found.

**Time Complexity**: O(n^3), where n is the length of `s`. This is because:
- O(n^2) substrings are generated.
- For each substring, O(n) work is required to check if it contains all characters of `t`.

**Space Complexity**: O(n) for storing substrings and their characters.

```javascript
function minWindowBruteForce(s, t) {
    if (s.length === 0 || t.length === 0) return "";

    let minLen = Infinity;
    let minStart = 0;
    let targetFrequency = Array(128).fill(0);

    // Populate target frequency array based on the string t
    for (let char of t) {
        targetFrequency[char.charCodeAt()]++;
    }

    for (let start = 0; start < s.length; start++) {
        let frequency = Array(128).fill(0);

        for (let end = start; end < s.length; end++) {
            frequency[s[end].charCodeAt()]++;
            
            if (containsAllCharacters(targetFrequency, frequency)) {
                if (end - start + 1 < minLen) {
                    minLen = end - start + 1;
                    minStart = start;
                }
                break;  // Once a valid substring has been found, stop to minimize it
            }
        }
    }

    return minLen === Infinity ? "" : s.substring(minStart, minStart + minLen);
}

function containsAllCharacters(targetFrequency, frequency) {
    for (let i = 0; i < 128; i++) {
        if (targetFrequency[i] > frequency[i]) {
            return false;
        }
    }
    return true;
}
```

---

### Sliding Window Optimal Approach

**Intuition**:  
The sliding window technique efficiently expands and contracts the window to find the smallest valid window. The idea is to use two pointers, one to expand the window (`end`) and one to shrink it (`start`). We maintain frequency counts of characters within the current window and compare them against the frequency counts needed from `t`.

**Steps**:
1. Record the frequencies of each character in `t`.
2. Use two pointers (`start` and `end`) to represent the window. Initialize `start` to 0 and `end` to 0.
3. Slide the `end` pointer to increase the size of the window and include new characters.
4. Once the window contains all characters of `t` (checked using a valid count and comparison with required frequencies), attempt to shrink it from `start` to reduce its size while maintaining validity.
5. Update the minimum window size when a smaller valid window is found.
6. Return the smallest valid window found.

**Time Complexity**: O(n), where n is the length of `s`. Each character is processed at most twice (once by `end`, and once by `start`).

**Space Complexity**: O(1), limited to 128 ASCII characters.

```javascript
function minWindow(s, t) {
    if (s.length === 0 || t.length === 0) return "";

    let targetFrequency = Array(128).fill(0);
    let windowFrequency = Array(128).fill(0);

    for (let char of t) {
        targetFrequency[char.charCodeAt()]++;
    }

    let start = 0, minStart = 0, minLen = Infinity, count = 0;

    for (let end = 0; end < s.length; end++) {
        let endChar = s[end].charCodeAt();

        // Include current character into window frequency
        if (targetFrequency[endChar] > 0) {
            windowFrequency[endChar]++;
            if (windowFrequency[endChar] <= targetFrequency[endChar]) {
                count++;
            }
        }
        
        // Check if current window contains all characters of t
        while (count === t.length) {
            if (end - start + 1 < minLen) {
                minLen = end - start + 1;
                minStart = start;
            }
            
            let startChar = s[start].charCodeAt();

            // Move start to find smaller window
            if (targetFrequency[startChar] > 0) {
                windowFrequency[startChar]--;
                if (windowFrequency[startChar] < targetFrequency[startChar]) {
                    count--;
                }
            }
            start++;
        }
    }

    return minLen === Infinity ? "" : s.substring(minStart, minStart + minLen);
}
```
This sliding window approach ensures efficient O(n) processing by dynamically adjusting the window size while ensuring that all characters from `t` are included in any solution windows evaluated.

