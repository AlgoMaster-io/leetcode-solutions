# [451. Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)

## Approach 1: Using HashMap and Sorting

### Solution
```javascript
// Time Complexity: O(n log n)
// Space Complexity: O(n)
function frequencySort(s) {
    // Count the frequency of each character
    const frequencyMap = new Map();
    for (const char of s) {
        frequencyMap.set(char, (frequencyMap.get(char) || 0) + 1);
    }

    // Sort characters by frequency in descending order
    const maxHeap = Array.from(frequencyMap.entries()).sort((a, b) => b[1] - a[1]);

    // Build the result string
    let result = '';
    for (const [char, freq] of maxHeap) {
        result += char.repeat(freq);
    }

    return result;
}
```

## Approach 2: Bucket Sort (Optimized for Large Inputs)

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(n)
function frequencySort(s) {
    // Count the frequency of each character
    const frequency = new Array(128).fill(0); // Assuming ASCII characters
    for (const char of s) {
        frequency[char.charCodeAt(0)]++;
    }

    // Create buckets where index represents frequency
    const buckets = Array.from({ length: s.length + 1 }, () => []);
    for (let i = 0; i < frequency.length; i++) {
        if (frequency[i] > 0) {
            buckets[frequency[i]].push(String.fromCharCode(i));
        }
    }

    // Build the result string from the buckets
    let result = '';
    for (let i = buckets.length - 1; i > 0; i--) {
        for (const char of buckets[i]) {
            result += char.repeat(i);
        }
    }

    return result;
}
```

