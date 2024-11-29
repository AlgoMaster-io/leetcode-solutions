# [451. Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)

## Approach 1: Using HashMap and Sorting

### Solution

typescript
```typescript
// Time Complexity: O(n log n)
// Space Complexity: O(n)
function frequencySort(s: string): string {
    // Count the frequency of each character
    const frequencyMap: Map<string, number> = new Map();
    for (const c of s) {
        frequencyMap.set(c, (frequencyMap.get(c) || 0) + 1);
    }

    // Sort characters by frequency in descending order
    const maxHeap: [string, number][] = Array.from(frequencyMap.entries()).sort((a, b) => b[1] - a[1]);

    // Build the result string
    const result: string[] = [];
    for (const [char, freq] of maxHeap) {
        for (let i = 0; i < freq; i++) {
            result.push(char);
        }
    }

    return result.join('');
}
```

## Approach 2: Bucket Sort (Optimized for Large Inputs)

### Solution

typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
function frequencySort(s: string): string {
    // Count the frequency of each character
    const frequency: number[] = new Array(128).fill(0); // Assuming ASCII characters
    for (const c of s) {
        frequency[c.charCodeAt(0)]++;
    }

    // Create buckets where index represents frequency
    const buckets: string[][] = Array.from({ length: s.length + 1 }, () => []);
    for (let i = 0; i < frequency.length; i++) {
        if (frequency[i] > 0) {
            buckets[frequency[i]].push(String.fromCharCode(i));
        }
    }

    // Build the result string from the buckets
    const result: string[] = [];
    for (let i = buckets.length - 1; i > 0; i--) {
        if (buckets[i].length) {
            for (const char of buckets[i]) {
                result.push(char.repeat(i));
            }
        }
    }

    return result.join('');
}
```

