# [Leetcode 767: Reorganize String](https://leetcode.com/problems/reorganize-string/)

## Approaches
- [Basic Approach: Frequency Count and Two-Pointer](#basic-approach-frequency-count-and-two-pointer)
- [Optimal Approach: Priority Queue (MaxHeap)](#optimal-approach-priority-queue-maxheap)

## Basic Approach: Frequency Count and Two-Pointer

### Intuition
The idea here is to first count the frequency of each character in the string. Then, sort characters based on their frequencies. Attempt to distribute characters evenly using a two-pointer technique to insert high-frequency characters at even indices (if possible), ensuring no two adjacent characters are the same. If the frequency of any character is more than `(n + 1) / 2`, where `n` is the length of the string, it's impossible to reorganize the string as required, because it would mean that even in the best case, some character would repeat consecutively.

### Steps
1. Count the frequency of each character in the string.
2. Sort characters based on frequency in descending order.
3. Attempt to distribute these characters starting from the first character and filling every second index to avoid adjacent duplicates.
4. If it's impossible to rearrange the string to avoid identical adjacent characters, return an empty string.

```javascript
/**
 * @param {string} S
 * @return {string}
 */
var reorganizeString = function(S) {
    const freqMap = new Map();
    
    // Count frequency of each character
    for (let char of S) {
        freqMap.set(char, (freqMap.get(char) || 0) + 1);
    }
    
    // Sort characters by frequency
    const sortedChars = Array.from(freqMap).sort((a, b) => b[1] - a[1]);
    
    const maxFreq = sortedChars[0][1];
    if (maxFreq > Math.ceil(S.length / 2)) {
        return ""; // Impossible to reorganize
    }
    
    const res = [];
    let idx = 0;
    
    // Place characters from the sorted list into the result array
    for (const [char, freq] of sortedChars) {
        for (let i = 0; i < freq; i++) {
            if (idx >= S.length) {
                idx = 1; // Start placing at odd indices
            }
            res[idx] = char;
            idx += 2;
        }
    }
    
    return res.join('');
};
```

### Time Complexity
- **O(n + k log k)**: `n` for the frequency count and `k log k` for sorting, where `k` is the number of unique characters.

### Space Complexity
- **O(n + k)**: `n` for the result and `k` for the auxiliary space for counting frequencies.

## Optimal Approach: Priority Queue (MaxHeap)

### Intuition
A more optimal approach uses a max-heap (priority queue) to always process the character with the highest remaining frequency. This minimizes the risk of two adjacent characters being the same immediately. We can use a greedy algorithm to place each character in the result string, maintaining a delay using temporary storage ("cool-off") to prevent consecutive duplicates.

### Steps
1. Count the frequency of each character.
2. Initialize a max-heap (or priority queue) to keep track of the most frequent characters.
3. Extract characters from the heap and append them to the result string, ensuring previously placed characters are put back into the heap once the "cool-off" condition is satisfied.
4. If at any point a character frequency exceeds the permissible `ceil(n/2)` limit, return an empty string since an arrangement will be impossible.

```javascript
/**
 * @param {string} S
 * @return {string}
 */
var reorganizeString = function(S) {
    const freqMap = new Map();
    
    // Count frequency of characters
    for (let char of S) {
        freqMap.set(char, (freqMap.get(char) || 0) + 1);
    }
    
    const maxHeap = [];
    
    // Build the max-heap based on character frequency
    for (let [char, freq] of freqMap) {
        maxHeap.push([freq, char]);
    }
    
    // Custom heap sort in descending order
    maxHeap.sort((a, b) => b[0] - a[0]);

    let prev = [-1, ''];
    let result = [];
    
    // Construct the result string
    while (maxHeap.length > 0) {
        let [freq, char] = maxHeap.shift();  // 'poll' the element with the highest frequency
        result.push(char);
        
        if (prev[0] > 0) {
            maxHeap.push(prev);
            maxHeap.sort((a, b) => b[0] - a[0]);  // Maintain max-heap order
        }
        
        // Update previous to be the current with reduced frequency
        prev = [freq - 1, char];
    }
    
    // If we can't complete the string with all characters, return empty
    if (result.length !== S.length) {
        return "";
    }
    
    return result.join('');
};
```

### Time Complexity
- **O(n log k)**: Priority queue operations for each character, where `n` is the string length and `k` is the number of unique characters.

### Space Complexity
- **O(k)**: For storing characters and their counts in the heap.

The optimal approach ensures a consistent way to resolve conflicts by leveraging the max-heap's ability to dynamically handle the most frequent entries.

