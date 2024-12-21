# [Leetcode 451: Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)

## Approaches:
1. [Frequency Map with Sorting](#approach-1-frequency-map-with-sorting)
2. [Frequency Map with Bucket Sort](#approach-2-frequency-map-with-bucket-sort)

---

### Approach 1: Frequency Map with Sorting

**Intuition:**

The basic idea is to count the frequency of each character in the given string and then sort the characters based on their frequencies in descending order. This can be efficiently achieved using a frequency map (object in JavaScript) and then sorting.

**Steps:**

1. **Count Frequencies:** Use a frequency map to count how often each character appears in the string.
2. **Sort Characters:** Convert the frequency map entries into an array and sort it primarily by frequency in descending order.
3. **Build Result String:** Construct the resulting string by repeating characters according to their frequency.

**Code:**

```javascript
function frequencySort(s) {
    // Step 1: Frequency Map
    const freqMap = {};
    for (let char of s) {
        freqMap[char] = (freqMap[char] || 0) + 1;  // count frequencies
    }

    // Step 2: Sort Characters by Frequency
    const sortedChars = Object.keys(freqMap).sort((a, b) => freqMap[b] - freqMap[a]);

    // Step 3: Build Result String
    let result = '';
    for (let char of sortedChars) {
        result += char.repeat(freqMap[char]);  // append the character freq[char] times
    }

    return result;
}
```

**Complexity Analysis:**

- **Time Complexity:** O(n + k log k)  
  - `O(n)` to build the frequency map
  - `O(k log k)` for sorting, where `k` is the number of unique characters.
- **Space Complexity:** O(n)
  - Frequency map can take up to O(n) space depending on the input size.

---

### Approach 2: Frequency Map with Bucket Sort

**Intuition:**

To achieve better performance in the scenario where we have a lot of characters, instead of sorting, we can use bucket sort. In this approach, we use an array of lists (buckets) where indices of the array represent frequencies, and lists at each index contain characters that appear with that frequency.

**Steps:**

1. **Count Frequencies:** Same as before using a frequency map.
2. **Create Buckets:** Use an array of length `(n+1)` (where n is the string length) to hold lists of characters. Each list at index `i` will contain characters with frequency `i`.
3. **Build Result String:** Traverse the buckets from highest frequency to lowest, constructing the result.

**Code:**

```javascript
function frequencySort(s) {
    // Step 1: Frequency Map
    const freqMap = {};
    for (let char of s) {
        freqMap[char] = (freqMap[char] || 0) + 1;
    }

    // Step 2: Create Buckets of Frequencies
    const maxFreq = s.length;
    const buckets = Array.from({ length: maxFreq + 1 }, () => []);
    for (let char in freqMap) {
        buckets[freqMap[char]].push(char);
    }

    // Step 3: Build Result String from Buckets
    let result = '';
    for (let i = maxFreq; i > 0; i--) {
        for (let char of buckets[i]) {
            result += char.repeat(i);  // append character i times
        }
    }

    return result;
}
```

**Complexity Analysis:**

- **Time Complexity:** O(n)  
  - Since we use bucket sort, which only requires linear time to fill and read buckets.
- **Space Complexity:** O(n)
  - The buckets array will take O(n) space, but it's typically smaller than storing characters directly multiple times.

This approach is particularly potent for handling strings with a large diversity of character frequencies efficiently.

