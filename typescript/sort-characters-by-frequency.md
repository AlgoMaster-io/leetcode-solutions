[Leetcode Problem 451: Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)

## Approaches

1. [Counting Frequency and Sorting](#approach-1-counting-frequency-and-sorting)
2. [Bucket Sort Method](#approach-2-bucket-sort)

### Approach 1: Counting Frequency and Sorting

**Intuition:**
The intuitive way to solve this problem is to first count the frequency of each character in the string. Then, sort these characters based on their frequencies in descending order, and finally, rebuild the string using the sorted order.

**Steps:**
1. First, we create a frequency map to keep track of the frequency of each character in the string.
2. Convert the frequency map into an array and sort it based on frequency in descending order.
3. Iterate over the sorted array, and rebuild the result string by repeating each character by its frequency.

```typescript
function frequencySort(s: string): string {
    // Step 1: Count the frequency of each character
    const frequencyMap: Map<string, number> = new Map();
    for (const char of s) {
        frequencyMap.set(char, (frequencyMap.get(char) || 0) + 1);
    }

    // Step 2: Sort characters by frequency
    const sortedChars: string[] = [...frequencyMap.entries()].sort((a, b) => b[1] - a[1]);
    
    // Step 3: Build the resulting string based on sorted frequencies
    let result: string = '';
    for (const [char, freq] of sortedChars) {
        result += char.repeat(freq);
    }

    return result;
}
```

**Time Complexity:** O(n + k log k), where n is the length of the string and k is the number of unique characters. Counting frequencies takes O(n), sorting them takes O(k log k).

**Space Complexity:** O(n), for storing the frequency map and the resulting string.

### Approach 2: Bucket Sort Method

**Intuition:**
To achieve optimal time complexity, we can use bucket sort. We will create an array (buckets) where the index represents the frequency, and each bucket stores characters with that frequency.

**Steps:**
1. Create a frequency map for the characters in the string.
2. Create an array of buckets, where each index corresponds to a frequency.
3. Place each character in the corresponding bucket based on its frequency.
4. Traverse the buckets from highest frequency to lowest and append characters to the result string.

```typescript
function frequencySort(s: string): string {
    // Step 1: Count the frequency of each character
    const frequencyMap: Map<string, number> = new Map();
    for (const char of s) {
        frequencyMap.set(char, (frequencyMap.get(char) || 0) + 1);
    }

    // Step 2: Create buckets based on frequency
    const maxFreq = Math.max(...frequencyMap.values());
    const buckets: string[][] = Array.from({ length: maxFreq + 1 }, () => []);

    for (const [char, freq] of frequencyMap.entries()) {
        buckets[freq].push(char);
    }

    // Step 3: Build the result string by iterating from high to low frequency
    let result: string = '';
    for (let i = maxFreq; i > 0; i--) {
        if (buckets[i].length > 0) {
            for (const char of buckets[i]) {
                result += char.repeat(i);
            }
        }
    }

    return result;
}
```

**Time Complexity:** O(n), where n is the length of the string. Counting frequencies, creating buckets, and constructing the result string all take O(n).

**Space Complexity:** O(n), for the frequency map and buckets array.

