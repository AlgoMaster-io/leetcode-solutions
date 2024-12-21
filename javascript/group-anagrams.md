[Leetcode Problem 49: Group Anagrams](https://leetcode.com/problems/group-anagrams/)

### Approaches:
1. [Basic Approach: Sort and Map](#basic-approach-sort-and-map)
2. [Optimal Approach: Count Vectors and Map](#optimal-approach-count-vectors-and-map)

---

### Basic Approach: Sort and Map

#### Intuition:
The basic idea is to use the sorted version of each string as a unique key. Anagrams, when sorted, will result in the same string. For example, "eat", "tea", and "ate" all become "aet". We can use this sorted string as the key in a map and group all relevant anagrams together.

#### Steps:
1. Initialize an empty map to store the grouped anagrams.
2. Iterate through each string in the input list:
   - Sort the string, which will serve as the key.
   - If the key is already in the map, append the original string to the list of its corresponding value.
   - If the key is not in the map, create a new entry with this key and initialize it with a list containing the original string.
3. The result is the collection of grouped anagrams from the map values.

#### Code:
```javascript
function groupAnagrams(strs) {
    // Initialize the map to store anagrams group.
    const map = new Map();
    
    for (let str of strs) {
        // Sort the string to use as a key.
        const sortedStr = str.split('').sort().join('');
        
        // If the sorted string is not already a key in the map, add it.
        if (!map.has(sortedStr)) {
            map.set(sortedStr, []);
        }
        
        // Append the original string to the list of the sorted key.
        map.get(sortedStr).push(str);
    }
    
    // Return all grouped anagrams.
    return Array.from(map.values());
}
```

#### Time Complexity:
- Sorting each string takes O(N log N) time, where N is the length of the string. If the average length of strings is L and we have M strings, it results in O(M * L log L).
  
#### Space Complexity:
- O(M * L), where M is the number of strings and L is the length of the strings in the input array, as we store all strings in the map.

---

### Optimal Approach: Count Vectors and Map

#### Intuition:
Instead of sorting the string (which is inefficient), we can count the occurrences of each character in the string. Two strings are anagrams if their character counts match. Use these counts as a key by converting them into a string.

#### Steps:
1. Initialize an empty map to store the grouped anagrams.
2. Iterate through each string in the input list:
   - Count the occurrences of each character (assuming lowercase alphabets).
   - Convert the count into a string and use it as a key in the map.
   - If this count key is already in the map, append the original string to its value.
   - If this count key is not in the map, create a new entry with this key.
3. Extract the values from the map to get the grouped anagrams.

#### Code:
```javascript
function groupAnagrams(strs) {
    // Initialize the map to store anagrams group with character count vectors as keys.
    const map = new Map();
    
    for (let str of strs) {
        // Initialize a character count array for 26 lowercase letters.
        const count = new Array(26).fill(0);
        
        // Count each character in the string.
        for (let char of str) {
            count[char.charCodeAt(0) - 'a'.charCodeAt(0)]++;
        }
        
        // Convert the count array into a string key.
        const key = count.join(',');
        
        // If the key is not in the map, initialize its entry with a list.
        if (!map.has(key)) {
            map.set(key, []);
        }
        
        // Append the original string to the list of the count key.
        map.get(key).push(str);
    }
    
    // Return all grouped anagrams.
    return Array.from(map.values());
}
```

#### Time Complexity:
- O(M * L), where M is the number of strings and L is the maximum length of a string. Counting characters is linear in each string's length.

#### Space Complexity:
- O(M * L), due to the storage in the map similar to the above approach. 

The optimal approach significantly reduces the overhead of sorting by replacing it with a linear character counting technique.

