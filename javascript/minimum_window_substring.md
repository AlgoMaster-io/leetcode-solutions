# [76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

## Approach 1: Brute Force (Basic)

### Solution
javascript
```javascript
// Time Complexity: O(n^2)
// Space Complexity: O(1)
function minWindow(s, t) {
    if (t.length > s.length) return "";

    let result = "";
    let minLength = Infinity;

    // Iterate over all substrings
    for (let i = 0; i < s.length; i++) {
        for (let j = i; j < s.length; j++) {
            let sub = s.substring(i, j + 1);

            if (containsAll(sub, t)) {
                if (sub.length < minLength) {
                    minLength = sub.length;
                    result = sub;
                }
            }
        }
    }

    return result;
}

function containsAll(sub, t) {
    let map = new Map();

    for (let char of t) {
        map.set(char, (map.get(char) || 0) + 1);
    }

    for (let char of sub) {
        if (map.has(char)) {
            map.set(char, map.get(char) - 1);
            if (map.get(char) === 0) {
                map.delete(char);
            }
        }
    }

    return map.size === 0;
}
```

## Approach 2: Sliding Window with Frequency Count (Optimal)

### Solution
javascript
```javascript
// Time Complexity: O(n)
// Space Complexity: O(n)
function minWindow(s, t) {
    if (t.length > s.length) return "";

    let tFreq = new Map();
    for (let char of t) {
        tFreq.set(char, (tFreq.get(char) || 0) + 1);
    }

    let windowFreq = new Map();
    let left = 0, right = 0, matched = 0;
    let minLength = Infinity;
    let start = 0;

    while (right < s.length) {
        let char = s[right];
        windowFreq.set(char, (windowFreq.get(char) || 0) + 1);

        if (tFreq.has(char) && windowFreq.get(char) === tFreq.get(char)) {
            matched++;
        }

        while (matched === tFreq.size) {
            // Update the result if this window is smaller
            if (right - left + 1 < minLength) {
                minLength = right - left + 1;
                start = left;
            }

            // Shrink the window
            let leftChar = s[left];
            if (tFreq.has(leftChar) && windowFreq.get(leftChar) === tFreq.get(leftChar)) {
                matched--;
            }
            windowFreq.set(leftChar, windowFreq.get(leftChar) - 1);
            if (windowFreq.get(leftChar) === 0) {
                windowFreq.delete(leftChar);
            }
            left++;
        }

        right++;
    }

    return minLength === Infinity ? "" : s.substring(start, start + minLength);
}
```

