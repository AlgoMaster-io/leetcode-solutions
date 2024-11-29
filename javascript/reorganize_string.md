# [767. Reorganize String](https://leetcode.com/problems/reorganize-string/)

## Approach 1: Max Heap

### Solution
```javascript
// Time Complexity: O(n log k), where k is the number of unique characters
// Space Complexity: O(n + k)
class Solution {
    reorganizeString(s) {
        // Frequency map for characters
        const charCounts = new Array(26).fill(0);
        for (const c of s) {
            charCounts[c.charCodeAt(0) - 'a'.charCodeAt(0)]++;
        }

        // Priority queue to keep the most frequent character at the top
        const maxHeap = [];
        for (let i = 0; i < 26; i++) {
            if (charCounts[i] > 0) {
                maxHeap.push([i, charCounts[i]]);
            }
        }

        // Custom comparator for max heap to sort by frequency
        maxHeap.sort((a, b) => b[1] - a[1]);

        let result = '';
        let prev = [-1, 0]; // Previously used character and its count

        while (maxHeap.length > 0) {
            const current = maxHeap.shift();
            result += String.fromCharCode(current[0] + 'a'.charCodeAt(0)); // Append current character
            current[1]--; // Decrease its count

            // Re-add the previous character if it's still valid
            if (prev[1] > 0) {
                maxHeap.push(prev);
                // Maintain heap property by re-sorting
                maxHeap.sort((a, b) => b[1] - a[1]);
            }

            // Update the previous character
            prev = current;
        }

        // If the result length is not the same as the input, return ""
        return result.length === s.length ? result : "";
    }
}
```

## Approach 2: Greedy with Frequency Array

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(1) (assuming a fixed alphabet size)
class Solution {
    reorganizeString(s) {
        const charCounts = new Array(26).fill(0);
        let maxCount = 0;
        let maxChar = '';

        // Count frequencies and find the most frequent character
        for (const c of s) {
            const index = c.charCodeAt(0) - 'a'.charCodeAt(0);
            charCounts[index]++;
            if (charCounts[index] > maxCount) {
                maxCount = charCounts[index];
                maxChar = c;
            }
        }

        // If the most frequent character cannot fit, return ""
        if (maxCount > (s.length + 1) / 2) {
            return "";
        }

        const result = new Array(s.length);
        let index = 0;

        // Place the most frequent character at even indices
        while (charCounts[maxChar.charCodeAt(0) - 'a'.charCodeAt(0)] > 0) {
            result[index] = maxChar;
            index += 2;
            charCounts[maxChar.charCodeAt(0) - 'a'.charCodeAt(0)]--;
        }

        // Place the remaining characters
        for (let i = 0; i < 26; i++) {
            while (charCounts[i] > 0) {
                if (index >= result.length) {
                    index = 1; // Start filling odd indices
                }
                result[index] = String.fromCharCode(i + 'a'.charCodeAt(0));
                index += 2;
                charCounts[i]--;
            }
        }

        return result.join('');
    }
}
```

