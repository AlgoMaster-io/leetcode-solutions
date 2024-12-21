# [Leetcode 49: Group Anagrams](https://leetcode.com/problems/group-anagrams/)

## Approaches
- [Approach 1: Sort each string and use as a key](#approach-1-sort-each-string-and-use-as-a-key)
- [Approach 2: Count Character Frequencies](#approach-2-count-character-frequencies)

---

## Approach 1: Sort each string and use as a key

### Intuition
The idea behind this solution is straightforward: anagrams, when sorted, will result in the same sequence of characters. Therefore, we can use sorted strings as keys in a hashmap (or an object in TypeScript) to group the original strings.

### Description
1. Initialize an object `anagrams` to store arrays of strings that are anagrams of each other.
2. Loop through each string in the input array.
3. Sort each string; the sorted form of an anagram group will always be the same.
4. Use the sorted string as a key in the `anagrams` object and push the original string into the corresponding array.
5. After processing all the strings, extract the values from the `anagrams` object, which will be the grouped anagrams.

### Code
```typescript
function groupAnagrams(strs: string[]): string[][] {
    const anagrams: { [key: string]: string[] } = {};

    for (const str of strs) {
        // Sort the string to use as a hash key
        const sortedStr = str.split('').sort().join('');

        // If the key doesn't exist, create a new array
        if (!anagrams[sortedStr]) {
            anagrams[sortedStr] = [];
        }
        
        // Add the original string to the corresponding key's array
        anagrams[sortedStr].push(str);
    }
    
    // Return all the grouped anagrams
    return Object.values(anagrams);
}
```

### Complexity
- **Time Complexity:** \(O(N \times K \log K)\), where \(N\) is the number of strings and \(K\) is the maximum length of a string. Sorting a string costs \(O(K \log K)\), and we do this for every string.
- **Space Complexity:** \(O(N \times K)\) for storing all the strings in the hashmap.

---

## Approach 2: Count Character Frequencies

### Intuition
Instead of sorting each string, we count the frequency of each character in the string. Arrays of character counts are used as keys. This can be more efficient since counting is \(O(K)\) compared to sorting which is \(O(K \log K)\).

### Description
1. Initialize an object `anagrams` to collect arrays of strings that are anagrams.
2. For each string, create a count of 26 zeroes to represent the count of each letter (assuming only lowercase English letters).
3. Convert this count to a string and use it as a key in the `anagrams` object.
4. Append the string to the array corresponding to this key.
5. Finally, extract the grouped anagrams from the `anagrams` object.

### Code
```typescript
function groupAnagrams(strs: string[]): string[][] {
    const anagrams: { [key: string]: string[] } = {};

    for (const str of strs) {
        const count = new Array(26).fill(0);
        
        // Count each character's frequency
        for (const char of str) {
            count[char.charCodeAt(0) - 'a'.charCodeAt(0)]++;
        }
        
        // Create a key from the count
        const key = count.join('#');
        
        if (!anagrams[key]) {
            anagrams[key] = [];
        }
        
        anagrams[key].push(str);
    }
    
    return Object.values(anagrams);
}
```

### Complexity
- **Time Complexity:** \(O(N \times K)\), where \(N\) is the number of strings and \(K\) is the maximum length of a string. Counting each character is \(O(K)\), and we do this for every string.
- **Space Complexity:** \(O(N \times K)\) for the hashmap and the output list.

